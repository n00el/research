# EvoSkill: Automated Skill Discovery for Multi-Agent Systems

**Authors:** Salaheddin Alzubi, Noah Provenzano, Jaydon Bingham, Weiyuan Chen, Tu Vu
**Date:** 3 Mar 2026
**Source:** https://arxiv.org/pdf/2603.02766
**Format:** PDF
**Pages:** 22

---

## Abstract

Coding agents are increasingly used as general-purpose problem solvers, but their flexibility does not by itself confer the domain expertise needed for specialized tasks. Recent work addresses this through *agent skills*: reusable workflows, and code, that augment agents with domain-specific capabilities. Most skills today are hand-crafted, and existing evolutionary approaches optimize low-level artifacts (e.g. prompts & code) that are tightly coupled to specific models and tasks. We introduce **EvoSkill**, a self-evolving framework that automatically discovers and refines agent skills through iterative failure analysis. EvoSkill analyzes execution failures, proposes new skills or edits to existing ones, and materializes them into structured, reusable skill folders. A Pareto frontier of agent programs governs selection, retaining only skills that improve held-out validation performance while the underlying model remains frozen. We evaluate EvoSkill on two benchmarks: OfficeQA, a grounded reasoning benchmark over U.S. Treasury data, where it improves exact-match accuracy by **7.3%** (60.6% -> 67.9%); and SealQA, a search-augmented QA benchmark with noisy retrieval, where it yields a **12.1%** gain (26.6% -> 38.7%). We also investigate the zero-shot transfer capabilities of skills evolved on one task to the other; in particular: skills evolved from SealQA transfers zero-shot to BrowseComp, improving accuracy by **5.3%** without modification demonstrating that skill-level optimization produces transferable capabilities beyond the training task.

## 1 Introduction

Coding agents (e.g., Claude Code[5], OpenHands [14], Codex[9]) have emerged as a dominant paradigm for solving tasks across a wide range of domains. This trend is driven by the increasing use of code as a flexible intermediate representation, enabling coding agents to invoke complex abstractions and operate as general-purpose problem solvers. However, while this flexibility allows agents to interface with diverse tools and domains, it does not by itself confer the domain expertise required to perform specialized tasks at a consistently high level.

To bridge this gap, recent work has explored *agent skills*: reusable, domain-specific capabilities that augment general-purpose coding agents with structured workflows, instructions, and supporting code. Most skills today are hand-crafted on an ad-hoc basis, requiring both domain knowledge and significant manual effort; a process that scales poorly as the number of target tasks grows. Evolutionary methods such as AlphaEvolve [8], and GEPA [2], offer a promising alternative by optimizing agent artifacts: codebases, or prompts, through iterative search. However, these approaches operate at the *artifact level*: the optimized prompts or code are tightly coupled to a specific model and task configuration, and do not naturally yield reusable components that transfer across settings.

In this work, we introduce **EvoSkill**, a self-evolving framework that operates at a higher level of abstraction: rather than optimizing prompts or code directly, EvoSkill iteratively *discovers and refines agent skills* through failure-driven textual feedback. EvoSkill maintains a Pareto frontier of agent programs and, at each iteration, analyzes execution failures to propose new skills or refine existing ones. Proposed skills are materialized into structured, reusable skill folders comprising instructions, trigger metadata, and helper scripts, and are retained only if they improve performance on a held-out validation set. Skills accumulate across iterations, progressively expanding the agent's capabilities while the underlying model remains frozen.

We validate EvoSkill across two benchmarks. On **OfficeQA**[11], a grounded reasoning benchmark over U.S. Treasury data, EvoSkill improves Claude Code with Opus 4.5 from 60.6% to **67.9%** exact-match accuracy (+7.3%) using only a small training subset. On **SealQA**[10], a search-augmented QA benchmark with noisy retrieval, EvoSkill yields a **12.1%** improvement (26.6% -> 38.7%). Furthermore, a skill evolved on SealQA transfers zero-shot to **BrowseComp**[15] with no modifications, improving accuracy by **5.3%** providing direct evidence that skills discovered by EvoSkill generalize beyond their training task.

Our contributions are as follows:

1. We propose **EvoSkill**, a framework for automatically discovering and refining reusable agent skills through iterative failure analysis, operating at the skill abstraction level rather than on low-level artifacts such as prompts or codebases.
2. We demonstrate that EvoSkill yields substantial improvements across two distinct benchmarks: **+7.3%** on OfficeQA [11](grounded document reasoning) and **+12.1%** on SealQA [10](search-augmented QA), using only small training subsets.
3. We show that skills evolved by EvoSkill transfer zero-shot to unseen tasks, with a skill discovered on SealQA improving BrowseComp accuracy by **5.3%** without modification; demonstrating that skill-level optimization produces transferable capabilities.

## 2 Methodology

The core idea behind EvoSkill is to iteratively discover and refine agent skills by applying textual feedback descent [6] to examples where the current agent fails. EvoSkill assumes a coding agent harness that supports skill folders (e.g., Claude Code, Codex, OpenCode) and a model capable of utilizing such skills. The underlying model remains frozen throughout; only the skill repository and agent metadata evolve across iterations.

### 2.1 Framework Overview

EvoSkill consists of three collaborating agents:

1. **Executor Agent** (*A*): Executes tasks under the governance of the current agent program. The base program initializes the Executor with no skills.
2. **Proposer Agent** (*P*): Analyzes the Executor's output traces, predicted answers, and ground-truth answers to diagnose failures and propose high-level skill descriptions. Ground-truth answers are provided to enable root-cause diagnosis, analogous to examining labeled misclassifications during error analysis in supervised learning, and are not propagated to the generated skills themselves. The Proposer determines whether to create a new skill or refine an existing one.
3. **Skill-Builder Agent** (*S*): Materializes a high-level proposal from the Proposer into a concrete skill folder comprising trigger metadata, procedural instructions (`SKILL.md`), and optional helper scripts (Python or Typescript code) or reference material. The Skill-Builder is bootstrapped with a meta-skill that codifies best practices for skill authoring.

All agents have read access to the base agent's repository; only the Skill-Builder has write permissions to the skills directory. The Proposer additionally maintains a cumulative **feedback history** *H* that logs all prior proposals, their outcomes, and score deltas. This serves two purposes: it prevents redundant proposals, and it enables the Proposer to refine what previously partially worked and avoid making the same mistakes making its context progressively richer across iterations.

### 2.2 EvoSkill Loop

EvoSkill optimizes agent programs through iterative skill mutations guided by textual feedback (Algorithm 1). A *program* *p* encapsulates the agent's system prompt and accumulated skills. The loop maintains a fixed-capacity frontier *G* of the *k* highest-scoring programs.

At each iteration *t*, a parent program *p* is selected from the frontier *G* via round-robin cycling, ensuring each frontier member is explored before any is revisited. The parent is evaluated on a training batch sampled without replacement, cycling through all examples before repeating. Responses are scored against ground-truth answers using a task-specific scoring function. Samples scoring below a fixed threshold are collected into a failure set *F*; if no failures are found, the iteration is skipped.

**Algorithm 1: EvoSkill -- Iterative Skill Induction via Textual Feedback**

**Require:** Executor agent *A*, proposer *P*, skill-builder *S*, evaluation set *V*, frontier capacity *k*, max iterations *T*

**Initialization:**
1. *H* <- []  (feedback history)
2. *G* <- {*A*}  (frontier of top-k programs)
3. *s_A* <- EVAL(*A*, *V*)  (baseline score)

**Evolution Loop:**
4. **for** *t* = 1 **to** *T* **do**
5.   *i* <- (*t* mod |*G*|); *p* <- *G*[*i*]  (round-robin parent selection)
6.   *F* <- COLLECTFAILURES(*p*)  (scores < tau)
7.   **if** *F* = {} **continue**
8.   **end if**
9.   *pi* <- *P*(*F*, *H*)  (propose new skill or edit)
10.  *p_tilde* <- *S*(*p*, *pi*)  (build candidate)

**Frontier Update:**
11.  *s_bar* <- EVAL(*p_tilde*, *V*)  (held-out score)
12.  **if** |*G*| < *k* **or** *s_bar* > min_{q in G} SCORE(*q*)
13.    *G* <- *G* union {*p_tilde*}  (Update frontier)
14.    **if** |*G*| > *k*  *G* <- *G* \ {arg min_{q in G} SCORE(*q*)}
15.    **end if**
16.  **end if**
17.  APPENDFEEDBACK(*H*, *pi*, *s_bar*)  (Update history log)
18. **end for**

**Output:**
19. **return** arg max_{q in G} SCORE(*q*)  (Return best performing program)

EvoSkill maintains a frontier *G* of the *k* best programs. Each iteration: a parent is selected round-robin from *G*, a proposer diagnoses its failures, a skill-builder produces a candidate, and the candidate enters the frontier if it outscores the weakest member.

The Proposer *P* receives *F* together with the feedback history *H* and performs structured failure analysis: reviewing execution traces, identifying capability gaps, and auditing existing skills. It then produces a textual proposal *pi* specifying either a new skill or an edit to an existing one.

The Skill-Builder *S* receives the current parent program *p* and proposal *pi*, and materializes the proposal into a candidate program *p_tilde*: the parent's configuration augmented with the new or revised skill. The candidate is evaluated on the held-out validation set *V* using the same scoring function.

The candidate enters the frontier *G* if its score exceeds that of the weakest frontier member, displacing it; otherwise the candidate is discarded. Regardless of outcome, the proposal, its score, and the selection verdict are appended to *H*, ensuring the Proposer can reference which strategies succeeded or regressed and why. After *T* iterations, the loop returns the highest-scoring program in *G*.

### 2.3 Implementation Details

#### 2.3.1 Data Setup

Given a supervised dataset D = {(x_i, y_i)}_{i=1}^{N} and a scoring function, we first cluster the dataset into *K* categories using an LLM as a classifier, assigning each example to a single category. We then perform stratified partitioning into three disjoint subsets: a **training set** used for failure detection during evolution, a **validation set** used for scoring candidate programs (frontier selection), and a **held-out test set** comprising all remaining examples, which is never exposed during evolution and is used exclusively for final evaluation. Split ratios are configurable, with defaults ensuring every category is represented in both the training and validation partitions regardless of category size. Training data are organized as category-keyed pools to support the category-aware sampling used during evolution. Full details of the splitting procedure are provided in Appendix C.

#### 2.3.2 Environment Setup

EvoSkill operates within a git repository where the codebase is fixed. Each agent program is represented as a branch that diverges from its parent only in its skill folders and metadata (system prompt, lineage information, validation score). This design ensures that performance differences between programs are attributable solely to their evolved skills, while keeping program branches lightweight. Full details of the repository configuration are provided in Appendix D.

## 3 Experiments

We evaluate EvoSkill along three axes: (1) whether iterative skill evolution improves agent performance on challenging benchmarks, (2) what properties of the training setup influence skill quality, and (3) whether evolved skills transfer zero-shot to unseen tasks. We additionally present qualitative examples of discovered skills to illustrate the nature of the capabilities EvoSkill produces.

### 3.1 OfficeQA

#### 3.1.1 Benchmark

OfficeQA is a grounded reasoning benchmark built from U.S. Treasury Bulletins -- a corpus of approximately 89,000 pages spanning five decades of monthly and quarterly publications. Each bulletin is 100-200 pages of prose, complex tables, charts, and figures describing Treasury operations. The benchmark consists of 246 questions organized into easy and hard difficulty levels. Questions require locating and synthesizing information across an average of two bulletin documents, navigating dense tabular data, and performing basic quantitative reasoning. Human solvers average 50 minutes per question, with the majority of time spent locating relevant information across tables and figures within the corpus.

#### 3.1.2 Setup

All experiments use Claude Code with Opus 4.5 as the underlying model. Following the data setup described in Section 2.3.1.

Following the data setup described in Section 2.3.1, we partition the benchmark into three disjoint splits: a **training set** used for failure detection during evolution, a **validation set** of 17 examples (~7%) used for frontier selection, and a **held-out test set** comprising the remaining questions, which is never exposed during evolution. We evaluate three training set sizes: 5% (12 examples), 10% (24 examples), and 15% (36 examples); each evolved for 1.5 epochs. We additionally evaluate a **skill-merge** configuration, which combines unique skills discovered across independent runs into a single skill library; when skills overlap (identified by matching names or descriptions), we retain the version from the highest-performing run. All reported accuracies are computed on the held-out test partition unless otherwise noted. We use the fuzzy scoring function provided by OfficeQA, which computes a weighted match across five tolerance levels favoring exact matches. Full scoring details are provided in Appendix C.

#### 3.1.3 Results

Table 1 presents results across all configurations and tolerance levels. EvoSkill yields consistent improvements over the baseline across all settings. On exact match (0% tolerance), training on 5% of the data improves accuracy from 60.6% to 63.4% (+2.8%), while 10% training data yields 65.8% (+5.2%). Performance plateaus beyond 10%: the 15% split achieves 64.5%, slightly below the 10% run, suggesting diminishing returns or mild overfitting as training data grows.

| Train Split/Tol. | 0.00% | 0.10% | 1.00% | 5.00% | 10.00% |
|---|---|---|---|---|---|
| base | 60.6 | 66.3 | 72.8 | 77.2 | 79.7 |
| 5% | 63.4 | 67.4 | 74.3 | 77.6 | 80.1 |
| 10% | 65.8 | 69.9 | 76.4 | 80.5 | **82.5** |
| 15% | 64.5 | 69.9 | 75.6 | 79.3 | 81.3 |
| merge-unique | **68.1** | **70.8** | **77.1** | **80.5** | 82.4 |

Tolerance = allowable relative error. **Blue** = best in column.

The skill-merge configuration achieves the strongest result at **67.9%** exact match (+7.3% over baseline), outperforming every individual run. This indicates that skills discovered from independent runs are complementary: each run surfaces different failure modes and corresponding capabilities, and combining them produces a more complete skill library. The improvement pattern is consistent across tolerance levels, with gains ranging from 2.7-4.5% at stricter tolerances.

#### 3.1.4 Qualitative Analysis of Discovered Skills

To illustrate the nature of skills that EvoSkill discovers, we highlight two representative examples from the evolved skill library:

**Data Extraction Verification.** EvoSkill discovered a skill enforcing a rigorous protocol for extracting numerical data from Treasury Bulletin tables. The skill is triggered whenever the agent extracts any value from parsed tables, and addresses concrete failure modes identified during evolution: adjacent cell misreads, wrong metric selection, and incorrect time granularity. This skill emerged directly from the Proposer's analysis of cases where the agent retrieved values from neighboring cells or confused similar-sounding metrics.

**Quantitative Analysis Methodology.** A second skill provides structured methodology guidance for quantitative financial analysis, including risk calculations, forecasting, currency conversion, and statistical inference. The skill enforces mandatory validation checkpoints before computation, preventing systematic errors from wrong data transformations, date misalignment, or confusion between sample and population statistics.

These examples illustrate that EvoSkill discovers interpretable, domain-relevant skills that target specific failure modes rather than producing opaque optimizations. Additional examples of discovered skills are provided in Appendix A.

### 3.2 SealQA

#### 3.2.1 Benchmark

SealQA [10] is a challenge benchmark for evaluating search-augmented language models on fact-seeking questions where web search yields conflicting, noisy, or unhelpful results. Unlike OfficeQA, which tests grounded reasoning over a fixed document corpus, SealQA evaluates an agent's ability to navigate the open web under adversarial retrieval conditions. This makes it a complementary testbed for EvoSkill: the skills required are fundamentally different, centering on search strategy and source verification rather than document parsing and numerical extraction.

#### 3.2.2 Setup

We run EvoSkill on the seal-0 split of SealQA (111 questions) using Claude Code with Opus 4.5 and a 10% training split, following the partition methodology described in Section 2.3.1. The remaining questions not used for training or frontier selection form the held-out test set. Evolution is run for 1.5 epochs.

#### 3.2.3 Results

EvoSkill improves accuracy on SealQA from 26.6% to **38.7%**, an absolute gain of **12.1%**. Among the skills discovered, EvoSkill produced a *search-persistence-protocol* that enforces exhaustive search strategies before the agent commits to an answer. The skill requires term interpretation expansion, multi-source verification, and completeness checks, directly addressing the benchmark's core challenge of premature search termination when initial results are noisy or misleading. This result demonstrates that EvoSkill generalizes beyond document-grounded reasoning to search-intensive tasks, discovering qualitatively different skills suited to the target domain.

### 3.3 Zero-Shot Skill Transfer

A key design goal of EvoSkill is that evolved skills, being structured and interpretable, should transfer across tasks without modification. We test this by taking the *search-persistence-protocol* skill evolved on SealQA and applying it zero-shot (with no edits) to BrowseComp [15]: a benchmark for evaluating browsing agents on challenging fact-seeking questions with short, uniquely correct answers. We evaluate on a stratified sample of 128 examples.

Despite being evolved on a different benchmark with different questions and difficulty characteristics, the transferred skill improves accuracy from 43.5% to **48.8%**, an absolute gain of **5.3%**. This provides direct evidence that skills discovered by EvoSkill are not overfit to their training task: the search-persistence-protocol captures a general capability -- exhaustive search before committing to an answer -- that is broadly useful for fact-seeking tasks regardless of the specific benchmark. This result supports the hypothesis that optimizing at the skill level, rather than at the level of prompts or code, yields more transferable improvements.

## 4 Related Work

### 4.1 Agent Skills

The idea of augmenting agents with reusable, modular capabilities has roots in both embodied AI and software engineering. Voyager [13] introduced an ever-growing skill library of executable code for an LLM-powered Minecraft agent, where skills are stored as programs in a vector database and retrieved by semantic similarity. Skills in Voyager are discovered through an automatic curriculum and iterative prompting with environment feedback, enabling lifelong learning without parameter updates. More recently, the Agent Skills specification [1] has formalized skills as a portable, open format: each skill is a filesystem directory containing a `SKILL.md` file with metadata and procedural instructions, optionally bundled with helper scripts and reference materials. This format has been adopted across multiple agent harnesses including Claude Code, the Claude API, and third-party tools [4][3]. Skills in this paradigm leverage progressive disclosure: metadata is loaded at startup, instructions are read on demand, and scripts are executed without entering the context window enabling agents to maintain many skills with minimal context overhead. Despite the maturity of skill infrastructure, skills today are predominantly hand-authored. EvoSkill addresses this gap by automatically discovering and refining skills through iterative failure analysis, producing artifacts that conform to the same structured skill format.

### 4.2 Textual Feedback and Evolutionary Optimization

A growing body of work explores using natural-language feedback, rather than scalar rewards, to guide iterative improvement of LLM-generated artifacts. Self-Refine [7] demonstrated that a single LLM can improve its own outputs through a generate-critique-refine loop, achieving consistent gains across diverse tasks without additional training. However, Self-Refine operates on individual outputs and does not accumulate knowledge across iterations.

Feedback Descent [6] formalizes this intuition into a general optimization framework, showing that rich textual feedback from evaluators can drive sustained improvement across domains including molecular design, SVG optimization, and prompt engineering. Feedback Descent maintains a frontier of top-performing candidates and accumulates feedback history, enabling an editor LLM to make increasingly informed revisions. EvoSkill builds directly on this paradigm, applying textual feedback descent to the problem of skill discovery rather than artifact optimization.

On the evolutionary side, AlphaEvolve [8] uses an ensemble of LLMs to evolve entire codebases through an evolutionary loop grounded by automatic evaluation, achieving breakthroughs in algorithm discovery and infrastructure optimization at Google. GEPA [2] takes a similar evolutionary approach to prompt optimization within the DSPy framework, using reflective mutation and Pareto-based candidate selection to evolve textual components of complex systems. Both approaches demonstrate the power of evolutionary search with LLM-driven mutations, but they optimize low-level artifacts (code or prompts) that are tightly coupled to specific tasks and models.

EvoSkill differs from these approaches in its level of abstraction. Rather than evolving code or prompts directly, EvoSkill evolves *skills*: structured, reusable capability modules that persist across tasks. This distinction has practical consequences: evolved skills are interpretable, composable, and as we demonstrate transferable to new tasks without modification.

### 4.3 Transfer Learning in LLM Agents

Transfer learning has been extensively studied in the context of neural network fine-tuning, but its application to LLM-based agents remains nascent. In embodied settings, Voyager [13] showed that a skill library learned in one Minecraft world could be applied to solve novel tasks in a new world, providing early evidence that code-based skills can transfer across environments. More broadly, work on prompt transfer has explored whether optimized prompts generalize across tasks or models, with mixed results -- prompts optimized for one setting often degrade when the model or task distribution shifts.

EvoSkill offers a different angle on transfer: because skills are structured as self-contained folders with explicit trigger conditions and procedural instructions, they are decoupled from both the training task and the underlying model. Our experiments provide direct evidence of this: a search-persistence skill evolved on SealQA transfers zero-shot to BrowseComp, yielding a 5.3 percentage point improvement without any modification. This suggests that skill-level optimization may offer a more natural unit of transfer than prompt-level or code-level optimization, though broader investigation across more diverse task pairs is needed.

## 5 Conclusion

We presented EvoSkill, a self-evolving framework that automatically discovers and refines reusable agent skills through iterative failure analysis. By operating at the skill level rather than optimizing low-level artifacts such as prompts or codebases, EvoSkill produces structured, interpretable capabilities that accumulate over iterations and transfer across tasks. Our experiments demonstrate consistent improvements on two distinct benchmarks: OfficeQA (+7.3%) and SealQA (+12.1%) using only small training subsets, and we provide direct evidence of zero-shot skill transfer from SealQA to BrowseComp (+5.3%).

Several directions remain for future work. First, we aim to evaluate EvoSkill across a broader range of domains to better understand the generality of evolved skills and to characterize which skills emerge as *domain-general* (e.g., search persistence, verification protocols) versus *domain-specific* (e.g., treasury table extraction). Second, extending EvoSkill to multi-modal tasks where skills may need to coordinate across vision, code, and language presents a natural next step as coding agents increasingly operate over heterogeneous inputs. Third, the modular structure of evolved skills opens the possibility of building shared skill libraries, where skills discovered on one task can be browsed, composed, and reused by other agents and users. Finally, deeper investigation into the transferability of skills across tasks, models, and agent harnesses, will be critical for realizing the full potential of skill-level optimization as a paradigm for improving coding agents.

## References

[1] Agent Skills. Agent skills specification, 2025. URL https://agentskills.io/specification.

[2] Lakshya A Agrawal, Shangyin Tan, Dilara Soylu, Noah Ziems, Rishi Khare, Krista Opsahl-Ong, Arnav Singhvi, Herumb Shandilya, Michael J Ryan, Meng Jiang, Christopher Potts, Koushik Sen, Alexandros G. Dimakis, Ion Stoica, Dan Klein, Matei Zaharia, and Omar Khattab. Gepa: Reflective prompt evolution can outperform reinforcement learning, 2026. URL https://arxiv.org/abs/2507.19457.

[3] Salaheddin Alzu'bi, Baran Nama, Arda Kaz, Anushri Eswaran, Weiyuan Chen, Sarvesh Khetan, Rishab Bala, Tu Vu, and Sewoong Oh. Roma: Recursive open meta-agent framework for long-horizon multi-agent systems, 2026. URL https://arxiv.org/abs/2602.01848.

[4] Anthropic. Anthropic skills documentation, 2025. URL https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.

[5] Anthropic. Claude code overview, 2026. URL https://code.claude.com/docs/en/overview.

[6] Yoonho Lee, Joseph Boen, and Chelsea Finn. Feedback descent: Open-ended text optimization via pairwise comparison, 2025. URL https://arxiv.org/abs/2511.07919.

[7] Aman Madaan, Niket Tandon, Prakhar Gupta, Skyler Hallinan, Luyu Gao, Sarah Wiegreffe, Uri Alon, Nouha Dziri, Shrimai Prabhumoye, Yiming Yang, Shashank Gupta, Bodhisattwa Prasad Majumder, Katherine Hermann, Sean Welleck, Amir Yazdanbakhsh, and Peter Clark. Self-refine: Iterative refinement with self-feedback, 2023. URL https://arxiv.org/abs/2303.17651.

[8] Alexander Novikov, Ngan Vu, Marvin Eisenberger, Emilien Dupont, Po-Sen Huang, Adam Zsolt Wagner, Sergey Shirobokov, Borislav Kozlovskii, Francisco J. R. Ruiz, Abbas Mehrabian, M. Pawan Kumar, Abigail See, Swarat Chaudhuri, George Holland, Alex Davies, Sebastian Nowozin, Pushmeet Kohli, and Matej Balog. Alphaevolve: A coding agent for scientific and algorithmic discovery, 2025. URL https://arxiv.org/abs/2506.13131.

[9] OpenAI. Codex, 2026. URL https://openai.com/codex/.

[10] Thinh Pham, Nguyen Nguyen, Pratibha Zunjare, Weiyuan Chen, Yu-Min Tseng, and Tu Vu. Sealqa: Raising the bar for reasoning in search-augmented language models, 2025. URL https://arxiv.org/abs/2506.01062.

[11] The Mosaic Research Team. Introducing officeqa: A benchmark for end-to-end grounded reasoning, December 2025. URL https://www.databricks.com/blog/introducing-officeqa-benchmark-end-to-end-grounded-reasoning. Accessed: 2026-02-20.

[12] Tu Vu, Kalpesh Krishna, Salaheddin Alzubi, Chris Tar, Manaal Faruqui, and Yun-Hsuan Sung. Foundational autoraters: Taming large language models for better automatic evaluation, 2024. URL https://arxiv.org/abs/2407.10817.

[13] Guanzhi Wang, Yuqi Xie, Yunfan Jiang, Ajay Mandlekar, Chaowei Xiao, Yuke Zhu, Linxi Fan, and Anima Anandkumar. Voyager: An open-ended embodied agent with large language models, 2023. URL https://arxiv.org/abs/2305.16291.

[14] Xingyao Wang, Boxuan Li, Yufan Song, Frank F. Xu, Xiangru Tang, Mingchen Zhuge, Jiayi Pan, Yueqi Song, Bowen Li, Jaskirat Singh, Hoang H. Tran, Fuqiang Li, Ren Ma, Mingzhang Zheng, Bill Qian, Yanjun Shao, Niklas Muennighoff, Yizhe Zhang, Binyuan Hui, Junyang Lin, Robert Brennan, Hao Peng, Heng Ji, and Graham Neubig. Openhands: An open platform for ai software developers as generalist agents, 2025. URL https://arxiv.org/abs/2407.16741.

[15] Jason Wei, Zhiqing Sun, Spencer Papay, Scott McKinney, Jeffrey Han, Isa Fulford, Hyung Won Chung, Alex Tachard Passos, William Fedus, and Amelia Glaese. Browsecomp: A simple yet challenging benchmark for browsing agents, 2025. URL https://arxiv.org/abs/2504.12516.

## Appendix A: EvoSkill Generated Skills

### A.1 OfficeQA Skills

The following economic-timeseries-skill is a complex skill that contains both a SKILL.md file and relevant Python scripts to be called with it.

**skills/economic-timeseries-analysis/SKILL.md**

```
---
name: economic-timeseries-analysis
description: >
  Streamlined workflow for economic/financial time-series analysis tasks. Use this skill when
  performing multi-step economic data analysis involving inflation adjustment
  using CPI values,
  linear regression on time-series data, or analysis of nominal dollar values
  converted to real
  values. Triggers on tasks requiring Treasury data analysis, BLS CPI adjustments,
  economic trend
  analysis, or formatted regression output like [slope, intercept].
---

# Economic Time-Series Analysis

Standardized workflow for inflation adjustment and statistical analysis of economic data.

## Workflow Overview

1. **Gather & Structure Data** - Collect nominal values and CPI data with consistent period formatting
2. **Adjust for Inflation** - Apply CPI formula: 'Real = Nominal x (CPI_base / CPI_current)'
3. **Perform Analysis** - Run linear regression on inflation-adjusted values
4. **Format Output** - Present results as '[slope, intercept]' rounded to 2 decimal places

## Step 1: Data Collection

Gather these inputs:
- **Nominal values**: Dollar amounts by period (e.g., monthly Treasury data)
- **CPI values**: Consumer Price Index for each period (BLS CPI-U series)
- **Base period**: Reference period for inflation adjustment (e.g., "1970-03")

Structure data with consistent period format (YYYY-MM):

'''json
{
    "nominal_values": [
        {"period": "1970-03", "value": 275.52},
        {"period": "1970-04", "value": 318.44}
    ],
    "cpi_values": [
        {"period": "1970-03", "value": 38.8},
        {"period": "1970-04", "value": 39.0}
    ],
    "base_period": "1970-03"
}
'''

## Step 2: Inflation Adjustment

Apply the standard inflation adjustment formula:

'''
Real Value = Nominal Value x (CPI_base / CPI_current)
'''

Example calculation:
- Nominal: $318.44 (April 1970)
- CPI_base (March 1970): 38.8
- CPI_current (April 1970): 39.0
- Real = 318.44 x (38.8 / 39.0) = $316.81

## Step 3: Linear Regression

Run regression with period index as x-values (0, 1, 2, ...) and real values as y-values.

Use 'scripts/analyze_timeseries.py' for automated analysis:

'''bash
python scripts/analyze_timeseries.py input.json
'''

Input JSON format:
'''json
{
    "nominal_values": [...],
    "cpi_values": [...],
    "base_period": "1970-03",
    "analysis_type": "linear_regression"
}
'''

## Step 4: Format Output

Present regression results rounded to 2 decimal places:

'''
[slope, intercept]
'''

Example: '[44.00, 231.52]'

## Quick Reference

| Step | Action | Output |
|------|--------|--------|
| 1 | Structure data | JSON with periods, values |
| 2 | Apply CPI adjustment | Real values series |
| 3 | Linear regression | slope, intercept |
| 4 | Format | '[slope, intercept]' |

## Common Data Sources

- **Treasury Bulletin**: Monthly Statement of Public Debt
- **BLS CPI-U**: Bureau of Labor Statistics Consumer Price Index
```

**skills/economic-timeseries-analysis/scripts/analysis.py**

```python
#!/usr/bin/env python3
"""
Economic Time-Series Analysis Script

Performs inflation adjustment and linear regression on economic data.

Usage:
    python analyze_timeseries.py <data_file> [--output <output_file>]

Input JSON format:
{
    "nominal_values": [
        {"period": "1970-03", "value": 100.0},
        {"period": "1970-06", "value": 110.0}
    ],
    "cpi_values": [
        {"period": "1970-03", "value": 38.8},
        {"period": "1970-06", "value": 39.0}
    ],
    "base_period": "1970-03",
    "analysis_type": "linear_regression"
}

Output JSON format:
{
    "real_values": [
        {"period": "1970-03", "nominal": 100.0, "real": 100.0},
        {"period": "1970-06", "nominal": 110.0, "real": 109.44}
    ],
    "regression": {
        "slope": 44.00,
        "intercept": 231.52
    },
    "formatted_result": "[44.00, 231.52]"
}
"""

import json
import sys
from typing import Optional


def load_data(filepath: str) -> dict:
    """Load JSON data from file."""
    with open(filepath, 'r') as f:
        return json.load(f)


def validate_data(data: dict) -> tuple[bool, str]:
    """Validate input data structure."""
    required = ['nominal_values', 'cpi_values', 'base_period']
    for field in required:
        if field not in data:
            return False, f"Missing required field: {field}"

    if not data['nominal_values']:
        return False, "nominal_values cannot be empty"
    if not data['cpi_values']:
        return False, "cpi_values cannot be empty"

    return True, ""


def get_cpi_for_period(cpi_values: list, period: str) -> Optional[float]:
    """Get CPI value for a specific period."""
    for item in cpi_values:
        if item['period'] == period:
            return float(item['value'])
    return None


def adjust_for_inflation(nominal_values: list, cpi_values: list, base_period: str) -> list:
    """
    Adjust nominal values for inflation.

    Formula: Real Value = Nominal x (CPI_base / CPI_current)
    """
    cpi_base = get_cpi_for_period(cpi_values, base_period)
    if cpi_base is None:
        raise ValueError(f"Base period CPI not found: {base_period}")

    real_values = []
    for item in nominal_values:
        period = item['period']
        nominal = float(item['value'])

        cpi_current = get_cpi_for_period(cpi_values, period)
        if cpi_current is None:
            raise ValueError(f"CPI not found for period: {period}")

        real = nominal * (cpi_base / cpi_current)
        real_values.append({
            'period': period,
            'nominal': nominal,
            'real': round(real, 2)
        })

    return real_values


def linear_regression(values: list) -> tuple[float, float]:
    """
    Perform simple linear regression.

    Uses period index (0, 1, 2, ...) as x-values.
    Returns (slope, intercept) rounded to 2 decimal places.
    """
    n = len(values)
    if n < 2:
        raise ValueError("Need at least 2 data points for regression")

    # x values are indices 0, 1, 2, ...
    # y values are the real (inflation-adjusted) values
    y_values = [v['real'] for v in values]

    # Calculate means
    x_mean = (n - 1) / 2  # mean of 0, 1, ..., n-1
    y_mean = sum(y_values) / n

    # Calculate slope: sum((x-x_mean)(y-y_mean)) / sum((x-x_mean)^2)
    numerator = sum((i - x_mean) * (y - y_mean) for i, y in enumerate(y_values))
    denominator = sum((i - x_mean) ** 2 for i in range(n))

    if denominator == 0:
        slope = 0.0
    else:
        slope = numerator / denominator

    # Calculate intercept
    intercept = y_mean - slope * x_mean

    return round(slope, 2), round(intercept, 2)


def analyze(data: dict) -> dict:
    """Main analysis function."""
    # Validate
    valid, error = validate_data(data)
    if not valid:
        raise ValueError(error)

    # Adjust for inflation
    real_values = adjust_for_inflation(
        data['nominal_values'],
        data['cpi_values'],
        data['base_period']
    )

    # Perform analysis
    analysis_type = data.get('analysis_type', 'linear_regression')

    result = {
        'real_values': real_values
    }

    if analysis_type == 'linear_regression':
        slope, intercept = linear_regression(real_values)
        result['regression'] = {
            'slope': slope,
            'intercept': intercept
        }
        result['formatted_result'] = f"[{slope:.2f}, {intercept:.2f}]"

    return result


def main():
    if len(sys.argv) < 2:
        print("Usage: python analyze_timeseries.py <data_file> [--output <output_file>]")
        print("\nExample:")
        print("  python analyze_timeseries.py input.json")
        print("  python analyze_timeseries.py input.json --output results.json")
        sys.exit(1)

    input_file = sys.argv[1]
    output_file = None

    if '--output' in sys.argv:
        idx = sys.argv.index('--output')
        if idx + 1 < len(sys.argv):
            output_file = sys.argv[idx + 1]

    try:
        data = load_data(input_file)
        result = analyze(data)

        output = json.dumps(result, indent=2)

        if output_file:
            with open(output_file, 'w') as f:
                f.write(output)
            print(f"Results written to: {output_file}")
        else:
            print(output)

        # Print formatted result for easy extraction
        if 'formatted_result' in result:
            print(f"\nFormatted Answer: {result['formatted_result']}")

    except Exception as e:
        print(f"Error: {e}", file=sys.stderr)
        sys.exit(1)


if __name__ == "__main__":
    main()
```

### A.2 SealQA Skills

The following *search-persistence-protocol* skill contains comprehensive instructions for web-search for conclusive answers where conflicting sources may exist. SealQA is considered a challenging benchmark for many multi-agent systems and

**skills/search-persistence-protocol/SKILL.md**

```
---
name: search-persistence-protocol
description: Enforces exhaustive search strategies before concluding factual
    answers. Use this skill when answering questions requiring web research,
    questions with potentially ambiguous terms, or enumeration/counting questions.
    Prevents premature search termination by requiring term interpretation
    expansion, multi-source verification, and completeness checks.
---

# Search Persistence Protocol

Enforces thorough search coverage before concluding. The agent's most common failure mode is stopping searches too early.

## Rule 1: Term Interpretation Expansion

Before searching ambiguous terms, list ALL reasonable interpretations.

**Example expansions:**
- "Agency" -> government agencies, space agencies, news agencies, regulatory agencies, intelligence agencies
- "Most widely used" -> by active users, by market share, by downloads, by revenue
- "Popular" -> most followers, most engagement, most referenced
- "Best" -> highest rated, most sales, expert recommended

**Protocol:** Execute searches for EACH interpretation before narrowing. Do not assume the first interpretation is correct.

## Rule 2: Three-Source Minimum

Before concluding ANY factual answer:
1. Check at least THREE independent sources
2. If only 1-2 sources found, search with different query formulations
3. For enumeration questions, cross-reference list against 2+ additional sources

**Trigger:** Before stating any factual claim, verify: "Have I checked 3+ sources?"

## Rule 3: "Unable to Find" Protocol

Before reporting inability to find data:

1. **Try 3+ query formulations** - Rephrase, use synonyms, try different term orders
2. **Try related searches** - Information may exist in adjacent contexts
3. **Attempt derivation** - Can the answer be calculated from related data?
    - If 96% have X, then ~4% don't have X
    - If list shows 11 items but "top 12" is mentioned, one is missing

**Rule 3a: Data Source Follow-Through** - If you identify a specific data source that should contain the answer (database URL, API endpoint, indicator code, dataset ID), you MUST attempt to fetch from it before concluding "unable to find."

- [X] "The ITU DataHub has indicator 100095 for 2G coverage. To get the exact figure, query the DataHub directly." -> WRONG: identified source but didn't fetch
- [OK] "The ITU DataHub has indicator 100095. Let me query it directly..." [proceeds to fetch] -> CORRECT

Anti-pattern: Telling the user "you would need to query X" when you could query X yourself.

Only report "unable to find" after exhausting these steps.

## Rule 4: Enumeration Completeness Check

When counting items in a category:

1. Search "[category] complete list" and "[category] all time"
2. Cross-reference count with 2+ ranking/list sources
3. Before finalizing: "Could I have missed any?"
4. If sources disagree on count, investigate the discrepancy

## Execution

For each factual question:

'''
1. EXPAND: List all interpretations of ambiguous terms
2. SEARCH: Query each interpretation separately
3. VERIFY: Confirm 3+ independent sources checked
4. COMPLETE: For enumerations, cross-check for missing items
5. DERIVE: If direct answer unavailable, attempt calculation from related data
6. CONCLUDE: Only after steps 1-5 are satisfied
'''
```

## Appendix B: Agent Prompts

### B.1 Proposer

**Proposer Agent Prompt**

```
"""
You are an expert agent performance analyst specializing in identifying
    opportunities to enhance agent capabilities through skill additions or
    modifications. Your role is to carefully analyze agent execution traces
    and propose targeted skill improvements.

## Your Task

Given an agent's execution trace, its answer, and the ground truth answer, propose
    either:
- A **new skill** (action="create") if no existing skill covers the capability gap
- An **edit to an existing skill** (action="edit") if an existing skill SHOULD
    have prevented the failure but didn't

Your proposal will be passed to a downstream Skill Builder agent for
    implementation.

## Required Pre-Analysis Steps

BEFORE proposing any skill, you MUST use the **Brainstorming skill** (read '.claude/skills/brainstorming/SKILL.md'):

1. **Use Brainstorming skill** (MANDATORY):
   - Read and follow the process in '.claude/skills/brainstorming/SKILL.md'
   - Propose 2-3 different approaches to address the failures
   - For each approach: describe the core idea, trade-offs, and complexity
   - Explore alternatives before settling on your final proposal
   - Apply YAGNI - choose the simplest solution that addresses the root cause

2. **Inventory existing skills**: Review the list of existing skills provided in
    the query
   - Understand what capabilities are already available
   - Check if any existing skill covers similar ground

3. **Analyze feedback history** for:
   - DISCARDED proposals similar to what you're considering
   - Patterns in what works vs what regresses scores
   - Skills that were active when failures occurred

4. **Determine action type**:
   - If an existing skill SHOULD have prevented this failure but didn't -> propose EDIT
   - If no existing skill covers this capability -> propose CREATE
   - If a DISCARDED proposal was on the right track -> explain how yours differs

## Analysis Process

Before proposing a solution, work through these steps:

<analysis>
1. **Trace Review**: Examine the agent's execution trace step-by-step
   - What actions did the agent take?
   - Where did it succeed or struggle?
   - What information was available vs. missing?

2. **Gap Analysis**: Compare the agent's answer to the ground truth
   - What specific information is incorrect or missing?
   - What reasoning errors occurred?
   - What capabilities would have prevented these issues?

3. **Existing Skill Check**: Review the listed existing skills
   - Does any existing skill cover this capability?
   - If yes, why did it fail to prevent the error?
   - Should that skill be EDITED instead of creating a new one?

4. **Skill Identification**: Determine what skill would address the failure
   - What new capability, tool, or workflow would help?
   - What inputs should it accept?
   - What outputs should it produce?
   - How would it integrate with existing capabilities?
</analysis>

## Anti-Patterns to Avoid

- DON'T propose a new skill if an existing one covers similar ground -> propose an EDIT instead
- DON'T ignore previous DISCARDED proposals for the same problem -> explain how yours differs
- DON'T create narrow skills that only fix one specific failure -> ensure broad applicability
- DON'T propose capabilities that overlap with existing skills -> consolidate instead

## When to Propose Skills

Propose a skill when ANY of these apply:
- Agent lacks access to information, APIs, or computational capabilities
- The fix requires a multi-step procedure (>3 sequential steps)
- The fix involves output structuring, formatting, or templates
- The improvement would be reusable across different tasks
- The issue is about WHAT steps to take, not HOW to think

## Output Requirements

Based on your analysis, provide:

1. **action**: Either "create" for a new skill or "edit" for modifying an existing skill

2. **target_skill**: (Required if action="edit") The name of the existing skill to modify

3. **proposed_skill**: A detailed description of:
   - For CREATE: The new skill to be built (capability, inputs, outputs, problem it solves)
   - For EDIT: The modifications needed to the existing skill

4. **justification**: Explain your reasoning
   - Reference specific moments in the trace that informed your decision
   - Reference specific existing skills and why they were/weren't suitable
   - Reference any related past iterations (especially DISCARDED ones)
   - Explain how your proposal addresses the identified gap

5. **related_iterations**: List of relevant past iterations (e.g., ["iter-4", "iter-9"])

## Example Analyses

<example type="edit_existing_skill">
**Situation**: Agent failed to calculate Expected Shortfall correctly. The
    financial-methodology-guide skill exists but didn't cover multi-period ES
    calculations.

**Proposal**:
- action: "edit"
- target_skill: "financial-methodology-guide"
- proposed_skill: "Extend the ES/CVaR section to include multi-period calculations.
    Add: (1) rolling window ES computation, (2) confidence interval adjustment
    for different time horizons, (3) examples showing ES at 1-day, 5-day, and 10-
    day horizons."
- justification: "The existing financial-methodology-guide skill covers basic ES
    but doesn't address the multi-period case seen in failure 1. Rather than
    creating a redundant skill, we should extend the existing methodology guide.
    Iter-3 was DISCARDED for proposing a separate 'multi-period-risk' skill -
    this proposal adds to the existing skill instead."
- related_iterations: ["iter-3"]
</example>

<example type="create_new_skill">
**Situation**: Agent failed to parse Treasury bond prices in 32nds notation. No
    existing skill covers notation parsing.

**Proposal**:
- action: "create"
- target_skill: null
- proposed_skill: "Create a 'bond-notation-parser' skill that handles Treasury
    bond price notation. It should: (1) recognize 32nds format (e.g., 99.16 = 99 +
    16/32), (2) handle the '+' suffix (e.g., 99.16+ = 99 + 16.5/32), (3) validate
    that fractional parts are 00-31, (4) provide clear examples and conversion
    formulas."
- justification: "No existing skill covers notation parsing. The trace shows the
    agent interpreted '99.16' as decimal 99.16 instead of 99.5 (99 + 16/32). This
    is a distinct capability not covered by existing methodology skills."
- related_iterations: []
</example>

<example type="data_access_skill">
**Situation**: Agent failed to answer a question about current stock prices
    because it only had access to historical data.

**Proposal**:
- action: "create"
- target_skill: null
- proposed_skill: "The agent needs a real-time stock price retrieval capability.
    This skill should accept a stock ticker symbol as input and return current
    market data including the latest price, daily change (absolute and percentage),
    and trading volume. It should handle invalid tickers gracefully and
    indicate whether markets are currently open or closed."
- justification: "At step 3 in the trace, the agent correctly identified the need
    for current pricing data and attempted to use its historical data tool.
    However, the ground truth required real-time information from today's trading
    session. The agent's reasoning was sound but it lacked the necessary data
    access. No existing skill provides real-time market data access."
- related_iterations: []
</example>
"""
```

### B.2 Skill-Builder

**Skill Builder Agent Prompt**

```
"""
You are an expert skill developer specializing in creating tools and capabilities
    for Claude Code agents. Your role is to implement well-structured, production-
    ready skills based on high-level descriptions provided by a Proposer agent.

## Primary Directive

**Before implementing any skill, always read and follow the '.claude/skills/skill-
    creator/SKILL.md' skill.** This skill contains essential patterns, validation
    requirements, scripts, and best practices that ensure your implementations
    work correctly within the Claude Code ecosystem. Follow its guidance for all
    skill creation tasks.

## Your Task

Given a proposed skill description, implement a complete, functional skill that:
1. Follows the skill-creator's structure and conventions
2. Integrates properly with the Claude Code SDK
3. Is well-documented and maintainable
4. Handles edge cases gracefully

## Implementation Process

Work through these steps for each skill implementation:

<implementation_steps>
1. **Read the Skill-Creator Skill**: Load and follow '.claude/skills/skill-creator/SKILL.md'

2. **Implement and Validate**: Build, test, and package the skill following skill-
    creator guidelines
</implementation_steps>

## Quality Reminder

The context window is a shared resource. Every token in your skill competes with
    conversation history, other skills, and user requests. Challenge each piece
    of content: "Does Claude really need this?" Keep skills concise and let
    Claude's intelligence fill in the gaps.
"""
```

### B.3 Auto-Grader

We use the default LLM-as-a-judge [12] template provided in the original SealQA task for all auto-grading tasks.

**LLM-Judge Prompt**

```
# Auto-Grader Prompt (Placeholder)

## System
You are the **Auto-Grader** agent. Evaluate a model output against a reference
    answer and scoring rubric.

## Inputs
- 'question': '<QUESTION>'
- 'prediction': '<MODEL_OUTPUT>'
- 'reference': '<REFERENCE_ANSWER>'
- 'tolerance': '<TOLERANCE>' (e.g., '0.01' for 1%)

## Output format
Return a JSON object with:
- 'score': '<FLOAT_0_TO_1>'
- 'passed': '<TRUE_FALSE>'
- 'reason': '<SHORT_EXPLANATION>'
- 'error_breakdown': [{'type': '<TYPE>', 'value': '<VALUE>'}, ...]

## Notes
- Prefer deterministic checks when possible.
- If numeric, compute relative error using placeholder formula:
  'rel_err = abs(p - r) / max(abs(r), eps)' with 'eps = 1e-9'.
```

## Appendix C: Scoring & Data Setup

We evaluate agent responses using a deterministic fuzzy matching scorer that returns a binary correctness signal (1.0 or 0.0). Given a ground-truth answer *g* and a predicted answer *p*, the scorer first attempts to extract numerical values from both strings along with surrounding textual context (a 20-character window). When both *g* and *p* contain numeric content, the scorer normalizes each extracted number by detecting unit keywords in the local context (e.g., "million," "billion," "trillion") and compares base values using a relative tolerance tau: a prediction is accepted if |g_base - p_base|/|g_base| <= tau. For the primary evaluation we set tau = 0, requiring exact numerical agreement. To avoid spurious matches against incidental year references (e.g., "reported in 2023"), the scorer filters candidate numbers in the 1900-2100 range from predictions unless the ground truth itself is a year or contains significant non-numeric text. For hybrid answers containing both text and numbers (e.g., "March 1977"), the scorer additionally verifies that key textual elements present in the ground truth appear in the prediction via case-insensitive substring matching after stripping unit words and parenthetical abbreviations. Multi-number ground-truth answers (i.e., lists) require that all constituent values are recovered in the prediction. For purely textual answers, matching is performed via case-insensitive substring containment after normalization (whitespace trimming, quote removal, and parenthetical stripping). During the self-improvement training loop, we additionally employ a multi-tolerance scoring variant that computes a weighted average over five tolerance levels tau in {0.0, 0.01, 0.025, 0.05, 0.10}, with weights w(tau) = 1/(1 + 20*tau) that favor stricter thresholds; a weighted score below 0.8 flags the example as a failure for targeted skill refinement.

## Appendix D: Environment Branches

We manage agent configurations -- which we term programs -- using a git-backed version control scheme that naturally encodes parent-child lineage and supports efficient frontier-based selection. Each program is stored on a dedicated git branch (prefixed program/) with a YAML configuration file (.claude/program.yaml) that records the program's name, a pointer to its parent branch, a generation counter (i.e., mutation depth from the root), the agent's system prompt, allowed tools, and evaluation metadata including scores. At initialization, a base program is created at generation 0 with no parent. At each iteration of the self-improvement loop, the system selects the highest-scoring program from a maintained frontier -- a bounded set of top-performing programs tracked via git tags (prefixed frontier/) -- and designates it as the parent. The parent's configuration is then mutated by invoking a proposer agent that analyzes sampled failures, producing a child program that inherits all parent attributes (system prompt, tool permissions) while introducing a targeted modification: either a new or edited skill file (in skill-only mode) or a rewritten system prompt (in prompt-only mode). The child is instantiated by checking out the parent branch, creating a new branch named iter-mode-n (where n is the global iteration index), writing the mutated configuration, and committing all changes. After the child is evaluated on a held-out validation set, it is admitted to the frontier if either the frontier has not reached its maximum capacity K (default K=3) or its score exceeds that of the current worst frontier member, in which case the weakest member is evicted. Children that fail to enter the frontier are discarded -- their branches are deleted to prevent repository bloat. This design yields a tree-structured search over program space in which each node is a fully reproducible snapshot: lineage can be reconstructed by following parent pointers from any program back to the root, and any historical configuration can be restored via a single git checkout. An early-stopping criterion halts the loop after a configurable number of consecutive iterations without frontier improvement.
