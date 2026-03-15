# Recursive Language Models

**Authors:** Alex L. Zhang, Tim Kraska, Omar Khattab
**Date:** January 29, 2026
**Source:** https://arxiv.org/pdf/2512.24601
**Format:** PDF
**Pages:** 38

---

## Abstract

We study allowing large language models (LLMs) to process arbitrarily long prompts through the lens of inference-time scaling. We propose **Recursive Language Models** (**RLMs**), a general inference paradigm that treats long prompts as part of an external *environment* and allows the LLM to *programmatically* examine, decompose, and *recursively call itself over* snippets of the prompt. We find that RLMs can successfully process inputs up to two orders of magnitude beyond model context windows and, even for shorter prompts, dramatically outperform the quality of vanilla frontier LLMs and common long-context scaffolds across four diverse long-context tasks while having comparable cost. At a small scale, we post-train the first natively recursive language model. Our model, **RLM-Qwen3-8B**, outperforms the underlying Qwen3-8B model by 28.3% on average and even approaches the quality of vanilla GPT-5 on three long-context tasks. Code is available at https://github.com/alexzhang13/rlm.

## 1. Introduction

Frontier reasoning models have limited context windows and, even within their limits, tend to exhibit *context rot* (Hong et al., 2025), a phenomenon illustrated in Figure 1 where quality degrades steeply as prompts get longer. Though we expect context lengths to steadily rise through improvements to training, architecture, and infrastructure, we are interested in *whether it is possible to scale the context size of general-purpose LLMs by orders of magnitude*. This is increasingly urgent as LLMs begin to be widely adopted for long-horizon tasks, in which they must routinely process tens if not hundreds of millions of tokens.

We study this question through the lens of scaling inference-time compute. We are inspired by the way that *reasoning models* have become the fundamental interface to LLMs, resulting not only in empirical gains but also additional theoretical expressive power (Merrill & Sabharwal, 2024) compared to vanilla Transformers. Though most inference-time methods for dealing with long context are task-specific (Wu et al., 2021; Chang et al., 2024), the most popular general approach is *context condensation* or *compaction* (Khattab et al., 2021; Smith, 2025; OpenAI, 2025b; Wu et al., 2025), where context from user requests or agent trajectories is repeatedly summarized once it exceeds a length threshold. Unfortunately, compaction is rarely expressive enough for tasks that require dense access throughout the prompt. It presumes that *some* details that appear early in the prompt can safely be forgotten to make room for new content.

We introduce **Recursive Language Models** (**RLMs**), a general-purpose inference paradigm for dramatically scaling the effective input and output lengths of LLMs. The key insight is that arbitrarily long user prompts should not be fed into the neural network (e.g., Transformer) directly but should instead be treated as *part of the environment that the LLM is tasked to symbolically and recursively interact with*.

As Figure 2 shows, an RLM exposes the same external interface as an LLM or a reasoning model: it accepts a string prompt of arbitrary structure and produces a string response. Given a prompt P, the RLM initializes a Read-Eval-Print Loop (REPL) programming environment in which P is set as the value of a variable. It then offers the LLM general context about the REPL environment (e.g., the length of the string P), and permits it to write code that peeks into and decomposes P, and to iteratively observe any side effects from execution. Crucially, RLMs encourage the LLM to understand, transform, and execute the input prompt by *writing symbolic programs that invoke the LLM itself* on as many slices of the input as necessary.

By treating the prompt itself as an external object and enabling symbolic recursion, RLMs tackle limitations of expressive power in recent work on coding agents, retrieval agents, and sub-agent delegation. In particular, prior coding agents and retrieval agents treat some designated external data source (e.g., a filesystem or a corpus of search documents) as an environment for fetching snippets. However, they *can only fill up the underlying LLM's context window with snippets before breaking down*. Similarly, prior self-delegation approaches (Anthropic, 2025; Sentient AI, 2025; Schroeder et al., 2025; Sun et al., 2025) allow LLMs to invoke themselves as sub-agents. However, they *are handicapped by the underlying LLM's limited output lengths* because they are designed to verbalize sub-calls autoregressively rather than producing them programmatically.

We evaluate RLMs using a frontier closed model (GPT-5; Singh et al. 2025) and a frontier open model (Qwen3-Coder-480B-A35B; Qwen Team 2025b) across four tasks with varying levels of complexity: deep research (Chen et al., 2025), information aggregation (Bertsch et al., 2025), code repository understanding (Bai et al., 2025), and a synthetic pairwise reasoning task where even frontier models fail catastrophically. We compare RLMs against direct LLM calls as well as context compaction, retrieval tool-use agents, and code-generation agents.

We find that RLMs demonstrate extremely strong performance even at the 10M+ token scale, and substantially outperform all other approaches at long-context processing, in many cases by double-digit percentage gains while maintaining comparable cost. In particular, as demonstrated in Figure 1, RLMs exhibit far less severe degradation for longer contexts and more sophisticated tasks.

Finally, at a small scale, we post-train the first natively recursive language model, demonstrating that RLMs can be improved quickly with little additional training. While a small open model (Qwen3-8B; Yang et al. 2025) struggles to solve long context tasks even in an RLM scaffold, our simple general-purpose training recipe uses only 1,000 samples from unrelated domains to improve its performance by a median of 28.3% across the four evaluation tasks.

## 2. Recursive Language Models

Given a base neural language model M with maximum context size K, a Recursive Language Model (RLM) is an inference-time scaffold around M that treats the user prompt as part of the environment without giving up the ability to densely process its content through different calls to M. Given an arbitrary-length prompt string P in Sigma*, an RLM interacts with a persistent external environment E and returns a response string Y in Sigma* (Figure 2). We would like effectively *unbounded input tokens* (|P| >> K), *unbounded output tokens*, and an *unbounded semantic horizon*, e.g. the ability to do Omega(|P|) or Omega(|P|^2) semantic work.

Algorithm 1 describes how an RLM achieves this. Given a prompt P, the RLM initializes a persistent REPL programming environment with a variable containing the user prompt as a string and a function for invoking a sub-RLM with a new prompt. Then, it starts the RLM loop. In the first iteration, the algorithm invokes the *root* neural model M with only (constant-size) metadata about the user prompt, like its length, a short prefix, and how to access parts of it.

The root is instructed via prompting (Appendix C) and/or fine-tuning (Appendix A) to operate like an RLM: that is, to *generate code that helps it understand and transform its parts of its prompt P*, and to build up intermediate values and the final response into new variables, potentially by *invoking the sub-RLM within loops*. In Section 4, we find that existing LLMs can be prompted to do this and that training an 8B model to be natively recursive is promising.

Each iteration of the RLM loop executes code in the REPL, updates REPL state (intermediate variables), and collects in stdout any printed text. Only (constant-size) metadata about stdout, like a short prefix and length, is appended to M's history for the next iteration. Once the RLM sets the variable Final inside the REPL, iteration stops and the value in Final is returned as the response.

RLMs make three simple design choices that are missing from existing scaffolds. To highlight these, we include Algorithm 2 to illustrate a deceptively "similar" algorithm that is far less expressive. Both algorithms support some notion of sub-calls, external objects, and code execution, but they differ in terms of where the prompt and intermediate values live and where recursion occurs.

First, an RLM must give the underlying LLM M a *symbolic handle* to the user prompt P, so the model can manipulate it without copying the full text into the root context window. Instead, ineffective Algorithm 2 starts by putting the user prompt P into the LLM context window (hist) and thus inherits the window limitations of M and falls back to heuristics like context compaction. Even though the scaffold can access external data with, say, a Search action or filesystem access, it is fatally bounded with respect to user input.

Second, ineffective Algorithm 2 asks M to autoregressively generate the output directly, via a Finish action. This may seem innocuous, but it means that it also cannot generate longer outputs than the context window of M permits.

Third, and perhaps most importantly, an RLM requires *symbolic recursion*. That is, code running *inside* E must be able to invoke M on programmatically constructed transformations of P (e.g., inside arbitrarily large loops), storing intermediate results symbolically. Though Algorithm 2 includes both a code execution action and a "sub-LLM" action separately, it is not able to invoke the sub-LLM programmatically and hence can only delegate a few *explicitly verbalized tasks* rather than writing short programs that can, say, loop over slices of the prompt and launch Omega(|P|) or even Omega(|P|^2) processes to understand or transform all parts of P.

### Algorithm 1: A recursive language model, around LLM M

**Input:** prompt P
**Output:** response Y

```
state <- InitREPL(prompt=P)
state <- AddFunction(state, sub_RLM_M)
hist <- [Metadata(state)]
while True do
    code <- LLM_M(hist)
    (state, stdout) <- REPL(state, code)
    hist <- hist || code || Metadata(stdout)
    if state[Final] is set then
        return state[Final]
```

### Algorithm 2: Alternate scaffold with standard (poor) design choices for prompts, sub-calls, and code execution

**Input:** prompt P
**Output:** response Y

```
actions <- {Finish, Exec, Search, sub_LLM_M}
hist <- [Metadata(actions), P]        // Flaw #1
while True do
    (action, val) <- LLM_M(hist)
    if action is Finish then
        return val                     // Flaw #2
    out <- RUN(action, val)            // Flaw #3
    hist <- hist || (action, val, out)
    if Tok(hist) > K then
        hist <- Compact(hist)
```

## 3. Scaling Long Context Tasks

We hypothesize that the effective context window (Hsieh et al., 2024; Goldman et al., 2025; Hong et al., 2025) of an LLM cannot be understood independently of the *specific task*. That is, more "complex" problems will exhibit degradation at even *shorter* lengths than simpler ones. Because of this, we must characterize tasks in terms of how their complexity *scales with prompt length*.

For example, needle-in-a-haystack (NIAH) problems generally keep 'needles' constant as prompt length is scaled. As a result, frontier models can now reliably solve these tasks in RULER (Hsieh et al., 2024) in the 1M+ token settings but struggle at far shorter lengths on OOLONG (Bertsch et al., 2025), a task where the answer depends explicitly on almost every line in the prompt.

### 3.1. Tasks

We design our evaluation around tasks where we can vary the lengths of the prompts, so we can consider problems whose difficulties scale differently with context length.

**S-NIAH.** Following the single needle-in-the-haystack task in RULER (Hsieh et al., 2024), we consider a set of 50 single tasks that require finding a specific phrase or number in a large set of unrelated text. Here, the information being sought scales as O(1) with respect to input length.

**BrowseComp-Plus (1K documents)** (Chen et al., 2025). A multi-hop question-answering benchmark for DeepResearch (OpenAI, 2025a) questions that requires reasoning over multiple different documents. The benchmark provides a verified offline corpus that is guaranteed to contain gold, evidence, and hard negative documents for each question. Following Sun et al. (2025), we use 150 randomly sampled instances as our evaluation set; we provide 1000 randomly chosen documents as input, in which the gold and evidence documents are guaranteed to exist. We report the percentage of correct answers. The answer to each task requires piecing together information from several documents, making this harder than **S-NIAH** despite also requiring a constant number of documents.

**OOLONG** (Bertsch et al., 2025). A long reasoning benchmark that requires transforming chunks of the input semantically, then aggregating these chunks to form a final answer. We report scoring based on the original paper, which scores numerical answers as score(y_hat) = 0.75^|y - y_hat| and other answers as exact match. We focus specifically on the trec_coarse split, a set of 50 tasks over a dataset of questions with semantic labels. Each task requires using nearly all entries of the dataset, and therefore scales linearly in processing complexity relative to the input length.

**OOLONG-Pairs.** We modify the trec_coarse split of OOLONG to include 20 new queries that specifically require aggregating *pairs* of chunks to construct the final answer. We report F1 scores over the answer. Each task requires using nearly all *pairs* of entries of the dataset, and therefore requires processing quadratically-many items relative to the input length. In Appendix D.1, we provide all queries in this benchmark.

**LongBench-v2 CodeQA** (Bai et al., 2025). A multi-choice code repository understanding split from LongBench-v2 that is challenging for modern models. We report the score as the percentage of correct answers. Each instance requires reasoning over a fixed number of files in a codebase to find the right answer.

### 3.2. Methods and Baselines

We compare RLMs against commonly used task-agnostic inference methods, using two modern LMs, GPT-5 with medium reasoning (Singh et al., 2025) and default sampling parameters, and Qwen3-Coder-480B-A35B (Yang et al., 2025) using the sampling parameters described in Qwen Team (2025b). For Qwen3-Coder-480B-A35B, we compute costs based on the compute provider Fireworks (Fireworks AI, 2025). In addition to evaluating the base model on all tasks, we also evaluate the following methods and baselines:

**CodeAct (+ BM25).** We compare directly to a CodeAct (Wang et al., 2024) agent that can execute code inside of a ReAct (Yao et al., 2023) loop. Unlike an RLM, CodeAct does not offload the user prompt to the code environment, and instead provides it directly to the LM. Furthermore, following Jimenez et al. (2024); Chen et al. (2025), we equip this agent with a BM25 (Robertson & Zaragoza, 2009) retriever that indexes the input context for tasks where a retriever is appropriate.

**CodeAct with sub-calls.** To specifically ablate offloading the context as a variable in the REPL, we evaluate a CodeAct (Wang et al., 2024) baseline with the ability to invoke sub-LM calls. Compared to RLMs, this method loads the context directly into the model.

**Summary agent.** Following Sun et al. (2025); Wu et al. (2025); Yu et al. (2025), we consider an iterative agent that compacts the context as it is filled. For example, given a corpus of documents, it will iteratively accumulate documents and summarize when full. In cases where a single document exceeds the model window, the agent will chunk it to fit within the model context window and invoke the same strategy over these chunks. For the GPT-5 experiments, due to the extremely high cost of applying this strategy to millions of tokens, we use GPT-5-nano for compaction and GPT-5 to provide the final answer.

**RLM with REPL.** We implement an RLM with a Python REPL environment, which loads a module for querying a sub-LM and uses a system prompt presented in Appendix C. For the GPT-5 experiments, we use GPT-5-mini for the recursive LMs and GPT-5 for the root LM, as we found this choice to strike a good balance between the capabilities of RLMs and the cost of the recursive calls. We notate a RLM using a model as RLM(model), e.g. RLM(GPT-5).

**RLM with REPL, no sub-calls.** We provide an ablation of our method, in which the prompt is loaded in a REPL environment without the ability to invoke sub-LM calls.

**Finetuning.** To create **RLM-Qwen3-8B**, we finetune Qwen3-8B on 1,000 filtered trajectories of Qwen3-Coder-480B-A35B as an RLM with Qwen3-8B sub-calls on LongBenchPro (Chen et al., 2026) tasks. We use sampling parameters described in Qwen Team (2025a), and evaluate the fine-tuned RLM-Qwen3-8B as an RLM on our long context tasks. The key insight for training is that being an effective sub-call model is roughly similar to being a general purpose reasoning model, so we can make the training much more tractable (and seemingly short-horizon) at small scale by focusing on improving the root model's ability to manipulate the REPL and to launch recursive calls. We provide training details more in Appendix A.

## 4. Results and Discussion

Table 1 reports our main results. We additionally explore how vanilla frontier model performance and RLM performance degrades as input contexts grow in Figure 1.

### Performance Comparison (Table 1)

Performance comparison of different methods across long-context benchmarks of varying complexity. Task lengths range from 23K-4.2M tokens (CodeQA) to 6M-11M tokens (BrowseComp+ 1K) to 131K tokens (OOLONG) to 32K tokens (OOLONG-Pairs).

**GPT-5** (with RLM sub-calls to GPT-5-mini):

| Model | CodeQA | BrowseComp+ (1K) | OOLONG | OOLONG-Pairs |
|---|---|---|---|---|
| Base Model | 24.0* | 0.0* | 44.0 | 0.1 |
| CodeAct (+ BM25) | 22.0* | 51.0 | 38.0 | 24.7 |
| CodeAct (+ sub-calls) | 24.0* | 0.0* | 40.0 | 28.4 |
| Summary agent | 58.0 | 70.5 | 46.0 | 0.1 |
| RLM | **62.0** | **91.3** | **56.5** | **58.0** |
| RLM (no sub-calls) | 58.0 | 88.0 | 36.0 | 43.9 |

**Qwen3-Coder-480B-A35B:**

| Model | CodeQA | BrowseComp+ (1K) | OOLONG | OOLONG-Pairs |
|---|---|---|---|---|
| Base Model | 20.0* | 0.0* | 36.0 | 0.1 |
| CodeAct (+ BM25) | 24.0* | 12.7 | 38.0 | 0.3 |
| CodeAct (+ sub-calls) | 26.0* | 0.0* | 32.0 | 0.1 |
| Summary agent | 50.0 | 38.0 | 44.1 | 0.31 |
| RLM | 56.0 | 44.7 | **48.0** | **23.1** |
| RLM (no sub-calls) | **66.0** | **46.0** | 43.5 | 17.3 |

**Qwen3-8B:**

| Model | CodeQA | BrowseComp+ (1K) | OOLONG | OOLONG-Pairs |
|---|---|---|---|---|
| Base Model | 4.0* | 0.0* | 0.0* | 0.1 |
| RLM | 26.0 | 2.0 | 24.0 | 4.3 |
| RLM (fine-tuned) | **32.0** | **14.0** | **32.0** | **5.2** |

(* indicates runs where a method sometimes ran into input context limits. Non-zero scores rounded to at least 0.1.)

**Observation 1: RLMs can scale to the 10M+ token regime and can outperform base LMs and existing task-agnostic agent scaffolds on long context tasks.** Across all tasks, RLMs demonstrate strong performance on prompts well beyond the effective context window of a frontier LM, outperforming base models and common long-context scaffolds by up to 2x the performance while maintaining comparable or cheaper average token costs. Notably, RLMs scale well beyond the base models' context window. For instance, on BrowseComp-Plus (1K), a linearly extrapolated cost for GPT-5-mini ingesting 6-11M input tokens is $1.50 - $2.75, while RLM(GPT-5) has an average cost of $0.99 and outperforms both the summarization and retrieval baselines by over 29%.

Furthermore, on tasks where processing costs scale with the input context, RLMs make significant improvements over the base model, even on tasks within the model's context window. On OOLONG, the RLM with GPT-5 and Qwen3-Coder outperform the base model by 28.4% and 33.3% respectively. On OOLONG-Pairs, both GPT-5 and Qwen3-Coder make little progress with F1 scores of <0.1%, while the RLM using these models achieve F1 scores of 58.0% and 23.1% respectively, highlighting the emergent capability of RLMs to handle extremely information-dense tasks.

**Observation 2: The REPL is necessary for handling long inputs, while the recursive sub-calling of RLMs provides strong benefits on information-dense inputs.** A key characteristic of RLMs is offloading the context as a variable in an environment E that the model can interact with. Even without sub-calling capabilities, our ablation of the RLM is able to scale beyond the context limit of the model and outperform other task-agnostic baselines on most long context settings. On the CodeQA and BrowseComp+ tasks with Qwen3-Coder, this ablation is able to outperform the RLM by 17.9% and 3% respectively.

On information-dense tasks like OOLONG or OOLONG-Pairs, we observed several cases where recursive LM sub-calling is necessary. In Section 4.1, we see RLM(Qwen3-Coder) perform the necessary semantic transformation line-by-line through recursive sub-calls, while the ablation without sub-calls is forced to use keyword heuristics to solve these tasks. Across all information-dense tasks, RLMs outperform the ablation without sub-calling by 10%-59%.

**Observation 3: LM performance degrades as a function of input length and problem complexity, while RLM performance scales better.** The benchmarks S-NIAH, OOLONG, and OOLONG-Pairs contain a fixed number of tasks over contexts with lengths ranging from 2^13 to 2^18. Each benchmark can be loosely categorized by different processing complexity of the input context with respect to length (roughly constant, linear, and quadratic respectively). In Figure 1, we directly compare an RLM using GPT-5 to base GPT-5 on each task. We find that GPT-5 performance degrades significantly faster for more complex tasks, while RLM performance degrades at a much slower rate, which aligns with the findings of Goldman et al. (2025). For context lengths beyond 2^14, the RLM consistently outperforms GPT-5.

Furthermore, RLM costs scale proportionally to the complexity of the task, while still remaining in the same order of magnitude of cost as GPT-5 (see Figure 11 in Appendix F). In Section 4.1, we explore the choices that the RLM makes that cause these differences in cost. Lastly, in this setting, we also observe that the base LM outperforms RLM in the small input context regime. By construction, a RLM has strictly more representation capacity than an LM. In practice, however, we observe that RLM performance is slightly worse on smaller input lengths, suggesting a tradeoff point between when to use a base LM and when to use an RLM.

**Observation 4: The inference cost of RLMs remains comparable to a base LM call but has high variance due to differences in trajectory lengths.** RLMs iteratively interact with their context until they find a suitable answer, leading to large differences in iteration length depending on task complexity. In Figure 3, we plot the quartile costs for each method across all experiments in Table 1 excluding BrowseComp+ (1K), as the base models cannot fit any of these tasks in context. For GPT-5, the median RLM run is cheaper than the median base model run, but many outlier RLM runs are significantly more expensive than any base model query. However, compared to the summarization agent which ingests the entire input context, RLMs are up to 3x cheaper while maintaining stronger performance across all tasks because the RLM is able to selectively view context.

**Observation 5: RLMs are a model-agnostic inference strategy, but different models exhibit different overall decisions on context management and sub-calling.** While GPT-5 and Qwen3-Coder-480B both exhibit strong performance as RLMs relative to their base model and other baselines, they also exhibit different performance and behavior across all tasks. On BrowseComp-Plus (1k) in particular, RLM(GPT-5) nearly solves all tasks while RLM(Qwen3-Coder) struggles to solve half.

We note that the RLM system prompt is fixed for each model across all experiments and is not tuned for any particular benchmark. Between GPT-5 and Qwen3-Coder, the only difference in the prompt is an extra line in the RLM(Qwen3-Coder) prompt warning against using too many sub-calls (see Appendix C). We provide an explicit example of this difference in example E.3, where RLM(Qwen3-Coder) launches a sub-call per line in OOLONG while GPT-5 is conservative about sub-querying LMs.

**Observation 6: Training RLMs on one domain can improve general downstream RLM performance.** Certain behavior in RLM trajectories are common among different domains, such as probing the input and recursively sub-calling on shorter contexts. In Table 1, we find that **RLM-Qwen3-8B**, a Qwen3-8B model that we fine-tuned on RLM(Qwen3-Coder-480B-A35B) trajectories on a small, *unrelated* set of tasks (LongBenchPro; Chen et al. 2026) considerably outperforms the base Qwen3-8B as an RLM by 28.3% on average. Furthermore, its inference costs are much lower due to better decision making and fewer mistakes as an RLM.

### 4.1. Emergent Patterns in RLM Trajectories

Even without explicit training, RLMs exhibit interesting context and problem decomposition behavior. We select several examples of snippets from RLM trajectories to understand how they solve long context problems and where they can improve. We discuss particular examples of interesting behavior here, with additional examples in Appendix E.

**Chunking and recursively sub-calling LMs.** RLMs defer essentially unbounded-length reasoning chains to sub-LM calls. The choice of decomposition can greatly affect task performance, especially for information-dense problems. In our experiments, we did not observe complicated partitioning strategies beyond uniform chunking or keyword searches. In Figure 4b, RLM(Qwen3-Coder) chunks by newline in a 1000+ line context from OOLONG.

**Filtering input information using code execution based on model priors.** A key intuition for why the RLM abstraction can maintain strong performance on huge inputs without exploding costs is the LM's ability to filter input context without explicitly seeing it. Furthermore, model priors enable the RLM to narrow the search space and process fewer input tokens. As an example, in Figure 4a, we observed RLM(GPT-5) using regex queries to search for chunks containing keywords in the original prompt (e.g. "festival") and phrases it has a prior about (e.g. "La Union").

**Passing recursive LM outputs through variables for long output tasks.** RLMs are able to produce essentially unbounded tokens well beyond the limit of the base LM by returning variables in the REPL as output. Through the REPL, the RLM can iteratively construct these variables as a mixture of programmatic and sub-(R)LM output calls. We observed this strategy used heavily in OOLONG-Pairs trajectories, where the RLM stored the output of sub-LM calls over the input in variables and stitched them together to form a final answer (see Figure 4c).

## 5. Related Works

**Long-Context LM Systems.** There have primarily been two orthogonal directions for long-context management in language model systems: 1) directly changing the architecture of and retraining the base LM to handle longer contexts (Press et al., 2022; Gu et al., 2022; Munkhdalai et al., 2024), and 2) building a scaffold around the LM that implicitly handles the context -- RLMs focus on the latter. One popular class of such strategies is *lossy* context management, which uses summarization or truncation to compress the input context at the cost of potentially losing fine-grained information. For example, MemWalker (Chen et al., 2023) constructs a tree-like data structure of the input that the LM can navigate when answering long context questions. ReSum (Wu et al., 2025) is another work that adds a summarization tool to periodically compress the context of a multi-turn agent. Another class of strategies implement an explicit memory hierarchy in the agent scaffold (Packer et al., 2024; Chhikara et al., 2025; Zhang et al., 2025). RLMs differ from these works in that all context window management is implicitly handled by the LM itself.

**Task Decomposition through sub-LM calls.** Many LM-based agents (Guo et al., 2024; Anthropic, 2025) use multiple, well-placed LM calls to solve a problem; however, many of these calls are placed based on human-engineered workflows. Several methods like ViperGPT (Suris et al., 2023), THREAD (Schroeder et al., 2025), DisCIPL (Grand et al., 2025), ReDel (Zhu et al., 2024), Context Folding (Sun et al., 2025), and AgentFold (Ye et al., 2025) have explored deferring the choice of sub-LM calls to the LM. These techniques emphasize *task decomposition* through recursive LM calls, but are unable to handle long context inputs beyond the length of the base LM. RLMs, on the other hand, are enabled by an extremely simple intuition (i.e., placing the prompt in the external environment) to *symbolically* manipulate arbitrarily long strings and to iteratively refine their recursion via execution feedback from the persistent REPL.

## 6. Limitations and Future Work

While RLMs show strong performance on tasks beyond the context window limitations of existing LMs at reasonable inference costs, evaluations for more difficult and natural long-context processing tasks and the best mechanisms for implementing RLMs both remain highly under-explored. We focused on synchronous sub-calls inside of a Python REPL environment, but we note that alternative strategies involving asynchronous sub-calls and sandboxed REPLs can potentially significantly reduce the runtime and inference cost of RLMs. Furthermore, we chose to use a max recursion depth of one (i.e. sub-calls are LMs); while we found strong performance on existing long-context benchmarks, we believe that future work should investigate deeper levels of recursion or even new hybrids between symbolic recursion and neural attention. We include additional limitations and negative results in Appendix B.

Lastly, we focused our experiments on evaluating RLMs using *existing* frontier models, but show initial evidence on a Qwen3-8B model that explicitly training a model to be used as a RLM provides very rapid performance improvements, even outside the training domain. We hypothesize that RLM trajectories can be viewed as a form of reasoning (OpenAI et al., 2024; DeepSeek-AI et al., 2025), which can be trained by bootstrapping existing models (Zelikman et al., 2022; 2024). We hope that training native RLMs can be treated as a new axis of scale to improve LM performance on general and long-horizon tasks.

## 7. Conclusion

We introduced Recursive Language Models (RLMs), a general inference framework for language models that offloads the input context and enables language models to recursively sub-query language models before providing an output. We explored an instantiation of this framework that offloads the context into a Python REPL environment as a variable in memory, enabling the LM to reason over its context in code and recursive LM calls, rather than purely in token space. Our results across multiple settings and models demonstrated that RLMs are an effective task-agnostic paradigm for both long-context problems and general reasoning. Building on our small fine-tuning experiments, we are excited to see future work that explicitly trains models to reason as RLMs, which could result in another axis of scale for the next generation of language model systems.

## 8. Impact Statement

This paper explores a strategy for enabling language models to solve long context problems and scaling future language model systems. The goal is to advance research on systems that can help us solve complex problems. While there are potential societal consequences of this work, we believe they are not specific to this paper and do not need to be highlighted here.

## Acknowledgments

This research is partially supported by the Laude Institute, Prime Intellect, and Modal Labs. We thank Noah Ziems, Jacob Li, James Moore, and the MIT OASYS and MIT DSG labs for insightful discussions throughout this project. We also thank Jack Cook, Matej Sirovatka, Ofir Press, Sebastian Muller, Simon Guo, and Zed Li for helpful feedback.

## References

Anthropic. Claude code: Subagents -- modular ai workflows with isolated agent contexts, 2025. URL https://docs.anthropic.com/en/docs/claude-code/sub-agents.

Bai, Y., Tu, S., Zhang, J., Peng, H., Wang, X., Lv, X., Cao, S., Xu, J., Hou, L., Dong, Y., Tang, J., and Li, S. J. Longbench v2: Towards deeper understanding and reasoning on realistic long-context multitasks. 2025. URL https://arxiv.org/abs/2412.15204.

Bertsch, A., Pratapa, A., Mitamura, T., Neubig, G., and Gormley, M. R. Oolong: Evaluating long context reasoning and aggregation capabilities. 2025. URL https://arxiv.org/abs/2511.02817.

Chang, Y., Lo, K., Goyal, T., and Iyyer, M. Booookscore: A systematic exploration of book-length summarization in the era of LLMs. In *The Twelfth International Conference on Learning Representations*, 2024. URL https://arxiv.org/pdf/2310.00785.pdf.

Chen, H., Pasunuru, R., Weston, J., and Celikyilmaz, A. Walking down the memory maze: Beyond context limit through interactive reading, 2023. URL https://arxiv.org/abs/2310.05029.

Chen, Z., Ma, X., Zhuang, S., Nie, P., Zou, K., Liu, R., Green, J., Patel, K., Meng, R., Su, M., Sharifymoghaddam, S., Li, Y., Hong, H., Shi, X., Liu, X., Thakur, N., Zhang, C., Gao, L., Chen, W., and Lin, J. Browsecomp-plus: A more fair and transparent evaluation benchmark of deep-research agent, 2025. URL https://arxiv.org/abs/2508.06600.

Chen, Z., Wu, X., Jia, J., Gao, C., Fu, Q., Zhang, D., and Hu, S. Longbench pro: A more realistic and comprehensive bilingual long-context evaluation benchmark, 2026. URL https://arxiv.org/abs/2601.02872.

Chhikara, P., Khant, D., Aryan, S., Singh, T., and Yadav, D. Mem0: Building production-ready ai agents with scalable long-term memory, 2025. URL https://arxiv.org/abs/2504.19413.

DeepSeek-AI, Guo, D., Yang, D., Zhang, H., Song, J., Zhang, R., Xu, R., Zhu, Q., Ma, S., Wang, P., Bi, X., Zhang, X., Yu, X., Wu, Y., Wu, Z. F., Gou, Z., Shao, Z., Li, Z., Gao, Z., Liu, A., Xue, B., Wang, B., Wu, B., Feng, B., Lu, C., Zhao, C., Deng, C., Zhang, C., Ruan, C., Dai, D., Chen, D., Ji, D., Li, E., Lin, F., Dai, F., Luo, F., Hao, G., Chen, G., Li, G., Zhang, H., Bao, H., Xu, H., Wang, H., Ding, H., Xin, H., Gao, H., Qu, H., Li, H., Guo, J., Li, J., Wang, J., Chen, J., Yuan, J., Qiu, J., Li, J., Cai, J. L., Ni, J., Liang, J., Chen, J., Dong, K., Hu, K., Gao, K., Guan, K., Huang, K., Yu, K., Wang, L., Zhang, L., Zhao, L., Wang, L., Zhang, L., Xu, L., Xia, L., Zhang, M., Zhang, M., Tang, M., Li, M., Wang, M., Li, M., Tian, N., Huang, P., Zhang, P., Wang, Q., Chen, Q., Du, Q., Ge, R., Zhang, R., Pan, R., Wang, R., Chen, R. J., Jin, R. L., Chen, R., Lu, S., Zhou, S., Chen, S., Ye, S., Wang, S., Yu, S., Zhou, S., Pan, S., Li, S. S., Zhou, S., Wu, S., Ye, S., Yun, T., Pei, T., Sun, T., Wang, T., Zeng, W., Zhao, W., Liu, W., Liang, W., Gao, W., Yu, W., Zhang, W., Xiao, W. L., An, W., Liu, X., Wang, X., Chen, X., Nie, X., Cheng, X., Liu, X., Xie, X., Liu, X., Yang, X., Li, X., Su, X., Lin, X., Li, X. Q., Jin, X., Shen, X., Chen, X., Sun, X., Wang, X., Song, X., Zhou, X., Wang, X., Shan, X., Li, Y. K., Wang, Y. Q., Wei, Y. X., Zhang, Y., Xu, Y., Li, Y., Zhao, Y., Sun, Y., Wang, Y., Yu, Y., Zhang, Y., Shi, Y., Xiong, Y., He, Y., Piao, Y., Wang, Y., Tan, Y., Ma, Y., Liu, Y., Guo, Y., Ou, Y., Wang, Y., Gong, Y., Zou, Y., He, Y., Xiong, Y., Luo, Y., You, Y., Liu, Y., Zhou, Y., Zhu, Y. X., Xu, Y., Huang, Y., Li, Y., Zheng, Y., Zhu, Y., Ma, Y., Tang, Y., Zha, Y., Yan, Y., Ren, Z. Z., Ren, Z., Sha, Z., Fu, Z., Xu, Z., Xie, Z., Zhang, Z., Hao, Z., Ma, Z., Yan, Z., Wu, Z., Gu, Z., Zhu, Z., Liu, Z., Li, Z., Xie, Z., Song, Z., Pan, Z., Huang, Z., Xu, Z., Zhang, Z., and Zhang, Z. Deepseek-r1: Incentivizing reasoning capability in llms via reinforcement learning, 2025. URL https://arxiv.org/abs/2501.12948.

Fireworks AI. Qwen3 coder 480b a35b instruct. https://fireworks.ai/models/fireworks/qwen3-coder-480b-a35b-instruct, 2025.

Goldman, O., Jacovi, A., Slobodkin, A., Maimon, A., Dagan, I., and Tsarfaty, R. Is it really long context if all you need is retrieval? towards genuinely difficult long context nlp, 2025. URL https://arxiv.org/abs/2407.00402.

Grand, G., Tenenbaum, J. B., Mansinghka, V. K., Lew, A. K., and Andreas, J. Self-steering language models. *arXiv preprint arXiv:2504.07081*, 2025.

Gu, A., Goel, K., and Re, C. Efficiently modeling long sequences with structured state spaces, 2022. URL https://arxiv.org/abs/2111.00396.

Guo, T., Chen, X., Wang, Y., Chang, R., Pei, S., Chawla, N. V., Wiest, O., and Zhang, X. Large language model based multi-agents: A survey of progress and challenges, 2024. URL https://arxiv.org/abs/2402.01680.

Hong, K., Troynikov, A., and Huber, J. Context rot: How context degradation affects llm performance, 2025. URL https://research.trychroma.com/context-rot.

Hsieh, C.-P., Sun, S., Kriman, S., Acharya, S., Rekesh, D., Jia, F., Zhang, Y., and Ginsburg, B. Ruler: What's the real context size of your long-context language models?, 2024. URL https://arxiv.org/abs/2404.06654.

Intellect, P. Prime rl library, 2025. URL https://github.com/PrimeIntellect-ai/prime-rl.

Jimenez, C. E., Yang, J., Wettig, A., Yao, S., Pei, K., Press, O., and Narasimhan, K. Swe-bench: Can language models resolve real-world github issues?, 2024. URL https://arxiv.org/abs/2310.06770.

Khattab, O., Potts, C., and Zaharia, M. Baleen: Robust multi-hop reasoning at scale via condensed retrieval. *Advances in Neural Information Processing Systems*, 34: 27670--27682, 2021.

Merrill, W. and Sabharwal, A. The expressive power of transformers with chain of thought. In *The Twelfth International Conference on Learning Representations*, 2024.

Munkhdalai, T., Faruqui, M., and Gopal, S. Leave no context behind: Efficient infinite context transformers with infini-attention, 2024. URL https://arxiv.org/abs/2404.07143.

OpenAI. Deep research, 2025a. URL https://openai.com/index/introducing-deep-research/. AI-powered research assistant tool.

OpenAI. Codex cli: A lightweight coding agent for your terminal, 2025b. URL https://developers.openai.com/codex-cli/.

OpenAI, Jaech, A., Kalai, A., Lerer, A., Richardson, A., El-Kishky, A., Low, A., Helyar, A., Madry, A., Beutel, A., Carney, A., Iftimie, A., Karpenko, A., Passos, A. T., Neitz, A., Prokofiev, A., Wei, A., Tam, A., Bennett, A., [...] and Wang, Z. Openai o1 system card, 2024. URL https://arxiv.org/abs/2412.16720.

Packer, C., Wooders, S., Lin, K., Fang, V., Patil, S. G., Stoica, I., and Gonzalez, J. E. Memgpt: Towards llms as operating systems, 2024. URL https://arxiv.org/abs/2310.08560.

Press, O., Smith, N. A., and Lewis, M. Train short, test long: Attention with linear biases enables input length extrapolation, 2022. URL https://arxiv.org/abs/2108.12409.

Qwen Team. Qwen3-8b. https://huggingface.co/Qwen/Qwen3-8B, 2025a.

Qwen Team. Qwen3-coder-480b-a35b-instruct. https://huggingface.co/Qwen/Qwen3-Coder-480B-A35B-Instruct, 2025b.

Redmon, J. and Farhadi, A. Yolov3: An incremental improvement, 2018. URL https://arxiv.org/abs/1804.02767.

Robertson, S. and Zaragoza, H. The probabilistic relevance framework: Bm25 and beyond. *Found. Trends Inf. Retr.*, 3(4):333--389, April 2009. ISSN 1554-0669. doi: 10.1561/1500000019. URL https://doi.org/10.1561/1500000019.

Schroeder, P., Morgan, N., Luo, H., and Glass, J. Thread: Thinking deeper with recursive spawning, 2025. URL https://arxiv.org/abs/2405.17402.

Sentient AI. Roma: The backbone for open-source meta-agents, November 2025. URL https://blog.sentient.xyz/posts/recursive-open-meta-agent. Accessed: 2025-12-20.

Singh, A., Fry, A., Perelman, A., Tart, A., Ganesh, A., El-Kishky, A., McLaughlin, A., Low, A., Ostrow, A., Ananthram, A., Nathan, A., Luo, A., Helyar, A., Madry, A., Efremov, A., Spyra, A., Baker-Whitcomb, A., Beutel, A., Karpenko, A., Makelov, A., Neitz, A., Wei, A., Barr, A., Kirchmeyer, A., Ivanov, A., Christakis, A., Gillespie, A., Tam, A., Bennett, A., Wan, A., Huang, A., Sandjideh, A. M., Yang, A., Kumar, A., Saraiva, A., Vallone, A., Gheorghe, A., Garcia, A. G., Braunstein, A., Liu, A., Schmidt, A., Mereskin, A., Mishchenko, A., Applebaum, A., Rogerson, A., Rajan, A., Wei, A., Kotha, A., Srivastava, A., Agrawal, A., Vijayvergiya, A., Tyra, A., Nair, A., Nayak, A., Eggers, B., Ji, B., Hoover, B., Chen, B., Chen, B., Barak, B., Minaiev, B., Hao, B., Baker, B., Lightcap, B., McKinzie, B., Wang, B., Quinn, B., Fioca, B., Hsu, B., Yang, B., Yu, B., Zhang, B., Brenner, B., Zetino, C. R., Raymond, C., Lugaresi, C., Paz, C., Hudson, C., Whitney, C., Li, C., Chen, C., Cole, C., Voss, C., Ding, C., Shen, C., Huang, C., Colby, C., Hallacy, C., Koch, C., Lu, C., Kaplan, C., Kim, C., Minott-Henriques, C., Frey, C., Yu, C., Czarnecki, C., Reid, C., Wei, C., [...] and Wang, Z. Openai gpt-5 system card, 2025. URL https://arxiv.org/abs/2601.03267.

Smith, C. Openhands context condensation for more efficient ai agents, 2025. URL https://openhands.dev/blog/openhands-context-condensation-for-more-efficient-ai-agents.

Sun, W., Lu, M., Ling, Z., Liu, K., Yao, X., Yang, Y., and Chen, J. Scaling long-horizon llm agent via context folding, 2025. URL https://arxiv.org/abs/2510.11967.

Suris, D., Menon, S., and Vondrick, C. Vipergpt: Visual inference via python execution for reasoning. *Proceedings of IEEE International Conference on Computer Vision (ICCV)*, 2023.

Wang, X., Chen, Y., Yuan, L., Zhang, Y., Li, Y., Peng, H., and Ji, H. Executable code actions elicit better llm agents, 2024. URL https://arxiv.org/abs/2402.01030.

Wu, J., Ouyang, L., Ziegler, D. M., Stiennon, N., Lowe, R., Leike, J., and Christiano, P. Recursively summarizing books with human feedback, 2021. URL https://arxiv.org/abs/2109.10862.

Wu, X., Li, K., Zhao, Y., Zhang, L., Ou, L., Yin, H., Zhang, Z., Yu, X., Zhang, D., Jiang, Y., Xie, P., Huang, F., Cheng, M., Wang, S., Cheng, H., and Zhou, J. Resum: Unlocking long-horizon search intelligence via context summarization, 2025. URL https://arxiv.org/abs/2509.13313.

Yang, A., Li, A., Yang, B., Zhang, B., Hui, B., Zheng, B., Yu, B., Gao, C., Huang, C., Lv, C., Zheng, C., Liu, D., Zhou, F., Huang, F., Hu, F., Ge, H., Wei, H., Lin, H., Tang, J., Yang, J., Tu, J., Zhang, J., Yang, J., Yang, J., Zhou, J., Zhou, J., Lin, J., Dang, K., Bao, K., Yang, K., Yu, L., Deng, L., Li, M., Xue, M., Li, M., Zhang, P., Wang, P., Zhu, Q., Men, R., Gao, R., Liu, S., Luo, S., Li, T., Tang, T., Yin, W., Ren, X., Wang, X., Zhang, X., Fan, Y., Su, Y., Zhang, Y., Wan, Y., Liu, Y., Wang, Z., Cui, Z., Zhang, Z., Zhou, Z., and Qiu, Z. Qwen3 technical report, 2025. URL https://arxiv.org/abs/2505.09388.

Yao, S., Zhao, J., Yu, D., Du, N., Shafran, I., Narasimhan, K., and Cao, Y. React: Synergizing reasoning and acting in language models, 2023. URL https://arxiv.org/abs/2210.03629.

Ye, R., Zhang, Z., Li, K., Yin, H., Tao, Z., Zhao, Y., Su, L., Zhang, L., Qiao, Z., Wang, X., Xie, P., Huang, F., Chen, S., Zhou, J., and Jiang, Y. Agentfold: Long-horizon web agents with proactive context management, 2025. URL https://arxiv.org/abs/2510.24699.

Yu, H., Chen, T., Feng, J., Chen, J., Dai, W., Yu, Q., Zhang, Y.-Q., Ma, W.-Y., Liu, J., Wang, M., and Zhou, Y. H. Memagent: Reshaping long-context llm with multi-conv rl-based memory agent, 2025. URL https://arxiv.org/abs/2507.02259.

Zelikman, E., Wu, Y., Mu, J., and Goodman, N. D. Star: Bootstrapping reasoning with reasoning, 2022. URL https://arxiv.org/abs/2203.14465.

Zelikman, E., Harik, G., Shao, Y., Jayasiri, V., Haber, N., and Goodman, N. D. Quiet-star: Language models can teach themselves to think before speaking, 2024. URL https://arxiv.org/abs/2403.09629.

Zhang, G., Fu, M., Wan, G., Yu, M., Wang, K., and Yan, S. G-memory: Tracing hierarchical memory for multi-agent systems, 2025. URL https://arxiv.org/abs/2506.07398.

Zhu, A., Dugan, L., and Callison-Burch, C. Redel: A toolkit for llm-powered recursive multi-agent systems, 2024. URL https://arxiv.org/abs/2408.02248.

## Appendix A. Additional Training Details

We trained **RLM-Qwen3-8B** as a very small scale exercise in training the first natively recursive language model. We hypothesized that, though acting as an RLM appears to produce sophisticated behavior due to recursion, it can be sufficient to focus on improving the root LM's ability to interact with the programmatic representation of the prompt in the REPL and to discern when sub-calls are useful. In other words, while a typical RLM trajectory can be extremely long due to all of the sub-calls potentially launched (possibly Omega(|P|) for a prompt P), the leaf sub-calls are essentially general-purpose LLM requests and the major hurdle is learning to operate as the root model.

This simple insight allowed us to explore a similarly simple recipe for training. In particular, we sampled RLM trajectories from a larger language model (Qwen3-Coder-480B-A35B-Instruct; Qwen Team 2025b) and, after filtering, distilled them to a smaller model (Qwen3-8B; Qwen Team 2025a) from the same model family. We evaluated RLM(Qwen3-Coder-480B-A35B) on 750 English LongBenchPro (Chen et al., 2026) tasks, collecting a total of 2250 candidate trajectories.

We first remove trajectories that score exactly 0.0 on the benchmark or do not go beyond one turn, bringing it down to 1,072 candidate trajectories. We separated each root RLM turn (i.e. iteration) as a separate SFT sample consisting of an input (the full history) and output (the output the root LM gave at that step).

We then applied a filtering step to remove turns beyond the context limit of Qwen3-8B (we approximated this as 100k characters), and also applied an extra programmatic correction step to fix small template mistakes in RLM usage (e.g. outputting final answers, calling the REPL, etc.). To elaborate, we noticed that trajectories generated by Qwen3-Coder-480B-A35B had noticeable mistakes in following the RLM instructions, which hurt the performance of the distilled RLM-Qwen3-8B. For example, it would often mix FINAL(answer) with FINAL(variable in REPL). We added an extra programmatic fixing step to look for common templated mistakes and patch them, leading to much better performance in the final **RLM-Qwen3-8B**. In total, 16% of turns cleaned incorrectly used FINAL answers, and 13% of turns incorrectly called a variable from the REPL (i.e. FINAL_VAR) as a final answer. In Figure 5, we show pre- and post-filtering statistics for our training trajectories.

We used the prime-rl library (Intellect, 2025) for fine-tuning. We used a batch size of 64 for 300 training steps, training for 48 H100 hours. While this exceedingly simple training recipe was able to demonstrate substantial gains for our 8B model, we call on future work to investigate training native RLMs much more thoroughly. We expect that doing so at much larger scales in terms of model size, number and variety of examples, and number of (ideally on-policy and online) rollouts will be necessary to maximize the potential of RLMs.

## Appendix B. Negative Results: Things we Tried that Did Not Work

Drawing inspiration from Redmon & Farhadi (2018), we try to be descriptive about what tricks, quirks, and other relevant things failed and succeeded in a concise manner. Some observations are based on longer supplementary experiments, while others are based on small samples of results.

**Using the exact same RLM system prompt across all models can be problematic.** We originally wrote the RLM system prompt with in context examples for GPT-5, and tried to use the same system prompt for Qwen3-Coder, but found that it led to different, undesirable behavior in the trajectory. We had to add a small sentence to the RLM system prompt for Qwen3-Coder to prevent it from using too many recursive sub-calls.

**Models without sufficient coding capabilities struggle as RLMs.** Our instantiation of RLMs relies on the ability to reason through and deal with the context in a REPL environment. We found from small scale experiments that smaller models like Qwen3-8B (Yang et al., 2025) struggled without sufficient coding abilities.

**Thinking models without sufficient output tokens struggle as RLMs.** In addition to Qwen3-Coder-480B-A35B-Instruct, we also tried experimenting with Qwen3-235B-A22B as the RLM. While we found positive results from the base model (e.g. on OOLONG (Bertsch et al., 2025), performance jumped from 30% to 38%), the smaller gap compared to the evaluated models in the main experiments (Table 1) are due to multiple trajectories running out of output tokens while producing outputs due to thinking tokens exceeding the maximum output token length of an individual LM call.

**RLMs without asynchronous LM calls are slow.** We implemented all sub-LM queries naively as blocking / sequential calls, which caused our RLM experiments to be slow, especially compared to just the base model. We are confident that this can be resolved with a robust implementation.

**Depending on the model, distinguishing between a final answer and a thought is brittle for RLMs.** The current strategy for distinguishing between a "next turn" and a final answer for the RLM is to have it wrap its answer in FINAL() or FINAL_VAR() tags. Similar to intuition about structured outputs degrading performance, we also found the model to make strange decisions (e.g. it outputs its plan as a final answer). We added minor safeguards, but we also believe this issue should be avoided altogether in the future when models are trained as RLMs.

## Appendix C. Additional Methods and Baseline Details

### C.1. Prompts for Experiments

We focus on methods that are entirely task agnostic, so we fix our prompt for each method across all tasks. For the RLM prompt, the only difference between GPT-5 and Qwen3-Coder is an added line in the beginning that warns Qwen3-Coder not to use too many sub-LM calls -- we found in practice that without this warning, the model will try to perform a subcall on everything, leading to thousands of LM subcalls for basic tasks! For the fine-tuned Qwen3-8B experiment, we provide a slightly different prompt due to the differences in context window size of the smaller model (from 272k to 32k). In this section, we provide the system prompt used for all methods in Section 3.1 (other than the base model, which does not include a system prompt).

**(1a) The system prompt for RLM with REPL for GPT-5:**

> You are tasked with answering a query with associated context. You can access, transform, and analyze this context interactively in a REPL environment, which you are strongly encouraged to use as much as possible. You will be queried iteratively until you provide a final answer.
>
> Your context is a {context_type} with {context_total_length} total characters, and is broken up into chunks of char lengths: {context_lengths}.
>
> The REPL environment is initialized with:
> 1. A 'context' variable that contains extremely important information about your query. You should check the content of the 'context' variable to understand what you are working with. Make sure you look through it sufficiently as you answer your query.
> 2. A 'llm_query' function that allows you to query an LLM (that can handle around 500K chars) inside your REPL environment.
> 3. The ability to use 'print()' statements to view the output of your REPL code and continue your reasoning.
>
> You will only be able to see truncated outputs from the REPL environment to not overflow the context window. Use these variables as buffers to build up your final answer.
> Make sure to explicitly look through the entire context in REPL before answering your query. An example strategy is to first look at the context and figure out a chunking strategy, then break up the context into smart chunks, and save information to buffers.
>
> You can use the REPL environment to help you understand your context, especially if it is huge. Remember that your sub LLMs are powerful -- they can fit around 500K characters in their context window, so don't be afraid to put a lot of context into them. For example, a viable strategy is to feed 10 documents per sub-LM query. Analyze your input data and see if it is sufficient to just fit it in a few sub-LM calls!
>
> [Examples of REPL usage with chunking, keyword search, and markdown-based strategies]
>
> IMPORTANT: When you are done with the iterative process, you MUST provide a final answer inside a FINAL function when you have completed your task. Do not use these tags unless you have completed your task. You have two options:
> 1. Use FINAL(your final answer here) to provide the answer directly
> 2. Use FINAL_VAR(variable_name) to return a variable you have created in the REPL environment as your final output
>
> Think step by step carefully, plan, and execute this plan immediately in your response -- do not just say "I will do this" or "I will do that". Output to the REPL environment and recursive LLMs as much as possible. Remember to explicitly answer the original query in your final answer.

**(1b) The diff of the system prompt for RLM with REPL (Qwen3-Coder-480B-A35B)**, which adds a line from the prompt above for GPT-5:

> IMPORTANT: Be very careful about using 'llm_query' as it incurs high runtime costs. Always batch as much information as reasonably possible into each call (aim for around ~200K characters per call). For example, if you have 1000 lines of information to process, it's much better to split into chunks of 5 and call 'llm_query' on each chunk (200 calls total) rather than making 1000 individual calls. Minimize the number of 'llm_query' calls by batching related information together.

**(1c) The diff of the system prompt for RLM with REPL (Qwen3-8B)**, which has a few changes from the GPT-5 prompt due to differences in context length and similar sub-calling behavior as Qwen3-Coder-A35B:

> IMPORTANT: You have a total context window of approximately ~32k tokens. Be very careful about context length limits, so you must be conservative with how much context you send in each call.
>
> [Adjusted llm_query to handle around ~100k chars, roughly 32k tokens]
>
> [Sub LLMs have a ~32k token limit (approximately ~24k characters) -- be careful not to exceed this]
>
> IMPORTANT: Be very careful about using 'llm_query' as it incurs high runtime costs. Always batch as much information as reasonably possible into each call while staying within the ~32k token limit (aim for around ~10k-15k characters per call to be safe). For example, if you have 1000 lines of information to process, it's much better to split into chunks of 50-100 and call 'llm_query' on each chunk (10-20 calls total) rather than making 1000 individual calls. Minimize the number of 'llm_query' calls by batching related information together, but always respect the ~32k token limit.
>
> [FINAL_VAR(final_answer) instead of FINAL_VAR(variable_name)]

**(2) The system prompt for RLM with REPL (no sub-calls):**

Similar to the full RLM prompt but without the llm_query function. The REPL environment is initialized with only:
1. A 'context' variable
2. The ability to use 'print()' statements

Includes examples of using regex searches and keyword-based filtering over the context, as well as chunking strategies using only code execution (no sub-LM calls).

**(3a) The system prompt for CodeAct with BM25** (used for BrowseComp+):

> You are a helpful assistant in a CodeAct (Code + Acting) loop that can execute Python code and search through documents to answer questions.
>
> You must follow this format for each step:
> 1. THINK: Reason about what you need to do next
> 2. ACT: Take an action (either execute code or SEARCH)
>
> Available Actions:
> - Execute Python code: Write code in ```python code blocks
> - SEARCH(query): Search through documents for information using BM25 retrieval
> - Provide final answer: "ANSWER: [your answer]"

**(3b) The system prompt for CodeAct** (without BM25, for tasks other than BrowseComp+):

Similar to (3a) but without the SEARCH action, as there is nothing to index or retrieve.

### C.2. Summary agent baseline

The summarization agent baseline follows the scaffold presented in Sun et al. (2025); Wu et al. (2025); Yu et al. (2025), which also mimics how contexts are typically compressed in a multi-turn setting in agents like Claude Code (Anthropic, 2025). In an iterative fashion, the agent is given inputs until its context is full, at which point it is queried to summarize all relevant information and continue. If the agent is given a context in a single step that is larger than its model context window, it chunks up this context and performs the summarization process over these chunks.

For our GPT-5 baseline, we chose to use GPT-5-nano to perform summarization to avoid exploding costs. This explains the large discrepancy in cost in Table 1 between GPT-5 and Qwen3-Coder on BrowseComp+, where the summary agent using Qwen3-Coder is nearly 20x more expensive on average. On this task in particular, we found on a smaller set of 20 random samples that the performance between using GPT-5 and GPT-5-nano is comparable.

## Appendix D. Additional Benchmark Details

### D.1. OOLONG-Pairs Benchmark

To create OOLONG-Pairs, we synthetically generate 20 new tasks based on the ground-truth labels for the OOLONG (Bertsch et al., 2025) trec_coarse split for input contexts of length in [1024, 2048, 4096, 8192, 16384, 32768, 65536, 131072, 262144, 524288, 1048576].

**Ensuring quadratic scaling on OOLONG-Pairs.** We noticed that many tasks that aggregate over pairs of entries could actually be solved without looking at the pairs and only looking at each entry in a linear fashion (e.g. using the principle of inclusion-exclusion in set theory), so we explicitly created questions that ask for all pairs satisfying some properties.

**Tasks 1-20:** Each task requires listing all pairs of user IDs (no duplicate pairs, list lower ID first) where both users satisfy specific combinations of instance types (description and abstract concept, entity, human being, numeric value, location, abbreviation) with varying constraints on dates and label combinations. Tasks progressively increase in complexity from simple pair-finding (Tasks 1-5) to asymmetric constraints where one user must have specific labels and the other must have different specific labels (Tasks 11-20).

### D.2. Scaling Huge Document Corpuses in BrowseComp+

In addition to the BrowseComp+ (Chen et al., 2025) results for k = 1000 documents in Section 4, we also include a smaller set of results on a subset of 20 tasks from the original 150 to show how performance degrades as a function of input size. In our original experiments, the base LMs were unable to handle the input contexts, so we add results to show how they degrade. We include two new baselines, namely **ReAct w/ GPT-5 + BM25** (a variant of the CodeAct baseline without access to a code environment) and **GPT-5 + pre-query BM25** (GPT-5 on pre-queried documents).

**RLMs are able to scale well without performance degradation.** RLM(GPT-5) is the only model / agent able to achieve and maintain perfect performance at the 1000 document scale, with the ablation (no recursion) able to similarly achieve 90% performance. The base GPT-5 model approaches, regardless of how they are conditioned, show clear signs of performance dropoff as the number of documents increase.

**RLM inference cost scales reasonably.** The inference cost of RLMs on this setup scale log-linearly, and are reasonably bounded compared to other common strategies like ReAct + BM25. If we extrapolate the overall token costs of GPT-5 assuming it has an infinite context window, we observe that the inference cost of using RLM(GPT-5) is cheaper.

## Appendix E. Additional RLM Trajectories

In this section, we provide several example trajectories to highlight characteristics of frontier models as RLMs. Many of the trajectories are too long to fit in text, so we describe each step and show specific examples when relevant.

A few noticeable properties of these trajectories are that RLMs often make non-optimal choices despite their strong results in Section 3. For example, in Example E.2, we observed that the RLM with Qwen3-Coder carefully constructs its final answer through a mix of recursive sub-calls and code execution in the first iteration, but then discards this information and continues wasting sub-calls before not using these stored answers. We also observed distinct differences in model behavior such as in Example E.3, where we found Qwen3-Coder make hundreds to thousands of recursive sub-calls for a single simple task, while GPT-5 makes on the order of ten.

### E.1. RLM(GPT-5) on BrowseComp-Plus-Query_74

The total cost of this trajectory was **$0.079**. In this task, the agent must find the answer to a multi-hop query given a corpus of 1000 unique documents (8.3M total tokens) that contain evidence documents and negatives.

**Step 1.** GPT-5 (as the root LM) first decides to probe the 1000 document list with regex queries. It has some priors about these events (as shown from its particular choice of words it looks for), but it also looks for specific keywords in the prompt like "beauty pageant" and "festival".

**Step 2.** After running its regex queries, the root LM finds an interesting snippet on the chunk at index 6, so it launches a recursive LM call over this snippet to look for information relevant to the original query. The RLM is able to both store this information in a variable answer6, as well as print this information out for the root LM to see. The sub-call finds the answer is likely 'Maria Dalmacio' and stores this information back in the root LM's environment.

**Step 3.** After checking the information above, the root LM reasons that it has enough information to answer the query. The root LM chooses to check its answer again with two additional recursive LM calls to confirm that its answer aligns with this check. Finally, the root LM returns its final answer as 'Maria Dalmacio', which is the correct answer.

### E.2. RLM(Qwen3-Coder) on OOLONG-Pairs-Query_3

The total cost of this trajectory was **$1.12**. In this task, the agent must output all pairs of user IDs satisfying some set of properties given a list of entries (32k tokens total). This is both an information dense long input as well as long output task, making it particularly challenging for current LMs.

**Step 1.** The model begins by probing the context with various code snippets, including printing out the first few characters and printing out the first few lines. The model continues probing by splitting the input context by newline characters and checking roughly what the data format looks like. From the given format, the model chooses to first semantically classify the data using sub-LM calls over smaller chunks of the input (to avoid context rot and mistakes in larger contexts) and provides a sample back to the root LM.

Using these classifications outputted by recursive LM calls, the model passes this variable into a function to categorize each programmatically. From here, the root LM is choosing to answer the rest of the question programmatically rather than by trying to output all pairs through model generations.

The root LM specifically looks for instances satisfying the query and adds them to a variable of target users. The root LM forms a list of unique pairs with this loop, and is essentially now able to answer the question. The model has stored these pairs in a variable to be outputted at the end.

**Step 2.** By this point the model has already successfully extracted the answer. Interestingly however, as we observed frequently with Qwen3-Coder, the model will continue to repeatedly verify its answers. The model also attempts to return its answer wrapped in a 'FINAL_VAR()' tag, but it does not accept its answer. This is likely a consequence of a) not tuning the prompt specifically for this model and b) the model not being trained to act as an RLM.

**Steps 3-11.** The model repeats verification and re-generation processes multiple times before finally returning an answer. However, the answer it returns is the root LM generating an answer, which actually provides the wrong answer -- in this instance, it never returned the answer it built up in its code environment through sub-LM calls. This is an example of a case where the RLM failed.

### E.3. RLM(Qwen3-Coder) on OOLONG-Query_212

The total cost of this trajectory was **$0.38**. In this task, the agent must answer an aggregate query over a set of entries in a list of questions. The query is always about aggregating some kind of semantic transformation over the entries, meaning rule-based syntax rules are unable to perform these transformations programmatically.

**Step 1.** The model begins by probing the context with various code snippets. Qwen3-Coder differs from GPT-5 in how liberal it is in its use of sub-calls. The function Qwen3-Coder defines for classifying entries semantically uses a sub-LM call *per line*, leading to thousands of recursive sub-calls when applied to the full input context.

**Step 2.** After defining and testing several functions for running the above classification question over its input context, the root LM launches a long code execution call to classify and answer the query.

**Final.** The model concludes programmatically from the large number of sub-calls it performed in Step 2 that 'Answer: description and abstract concept is less common than numeric value' was the correct answer. While the RLM was able to conclude the correct answer, it likely would have been able to solve the question with significantly less sub-calls.

### E.4. RLM(GPT-5) on CodeQA-Query_44

The total cost of this trajectory was **$0.27**. In this task, the agent must answer a question that involves understanding a large codebase. The codebase here is ~900k tokens.

**Step 1.** It is not always true that an input context can be solved by partitioning it and recursively sub-querying models over each partition, but in tasks that are not information dense, this is possible. In this case, the model chooses to break the codebase into parts and sub-query LMs to look for clues. The model then aggregates these clues and provides a final answer as a separate sub-query.

**Final.** The RLM answers choice '1', which is the correct answer.

## Appendix F. Additional Runtime and Cost Analysis of RLMs

We supplement the cost and runtime analysis of RLMs with additional, fine-grained plots. In Figures 9, 10 we include a histogram for the cost of each method on every task for both GPT-5 and Qwen3-Coder. We generally observe long-tailed, high-variance trajectories for RLMs in both models.

We additionally include log-scaled runtime plots for each method below. As we remarked in Section 4.1, the runtime for these methods can be significantly improved through asynchrony of LM calls and additional prompting to discourage long sub-LM calls or code.

For the scaling plot in Figure 1, we also provide the average API cost per task.
