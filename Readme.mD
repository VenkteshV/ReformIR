<p align="center">
  <img src="reformir_logo.png" width="480" alt="ReformIR logo">
</p>

<h1 align="center">ReformIR</h1>
<p align="center"><b>When More Reformulations Hurt: Avoiding Drift using Ranker Feedback</b></p>

<p align="center">
  <a href="https://dl.acm.org/doi/10.1145/3805712.3809721">Paper</a> ·
  <a href="https://venkteshv.github.io/Adaptive_ReformIR/">Website</a> ·
  <a href="#citation">Citation</a>
</p>

---

This repo contains the code , generated reformulations (under data/) and run files (under runs/) to reproduce the main results.

## Overview

Query reformulation improves recall, but naively merging or exhaustively reranking many reformulations is expensive: and indiscriminate expansion causes **query drift**, where reformulated queries stray from the user's original intent.

**ReformIR** is a budget-aware retrieval framework that treats query reformulations as first-class features and learns which ones to trust using feedback from a strong reranker, anchored to the original query. Under a fixed reranking budget, it:

- Builds a large candidate pool from multiple reformulated queries
- Learns a lightweight **linear surrogate model** over reformulation-specific retrieval signals (BM25 per reformulation + RM3)
- Uses a **bandit-style optimization loop** to spend limited reranker calls on the most promising documents
- Anchors all reranker feedback to the **original query**, actively suppressing drift

The result: consistently higher recall than existing reformulation strategies, especially as the number of reformulations grows — a regime where prior methods degrade sharply.

## Repository structure

```
ReformIR/
├── code/                # Core pipeline: indexing, reformulation, surrogate model, run scripts
├── data/                # Pre-generated reformulations per method (GenQR, GenQREnsemble, Query2Doc, QA-Expand, ...)
├── runs/reform/          # Saved run files for reproducing paper tables without regenerating reformulations
└── requirements.txt
```


ReformIR helps reduce drift in reformulations by prioritzing reformulations and documents. ReformIR is also interpretable and
and example is given below:


# Steps
1. First create a conda environment : conda create -n ReformIR python=3.11
2. conda activate ReformIR
3. Install dependencies: pip install -r requirements.txt

```bash
1. First create a conda environment : conda create -n ReformIR python=3.11
2. conda activate ReformIR
3 pip install -r requirements.txt
```

# To reproduce results 
1. To reproduce the numbers in Tables 1 and 2 just specify the appropriate name in the run_dl19 or run_dl22 etc. script based on the test set you want to focus on.

For instance if you want to reproduce qa expand at c=50 on dl22 substitute the corresponding run file names that can be found under runs/ in names=[] array and set mode to "reuse". 


# To run the approaches from scratch without using runfiles 
1. Build indices: python3 code/build_indexes.py
2. The generated reformulations are given in data folder. Just provide the right path based on results you want to reproduce and change mode to overwrite from reuse.
3. The for instance if you want to reproduce genqr ensemble and genqr ensemble plus reform IR results from table 2 run: <br/>

```python3 code/run_dl22.py --dl 22 --ce 4 --budget 50 --reformulation_file=data/genqr_ensemble/dl22.csv``` <br/>

for budget 100 <br/>
```python3 code/run_dl22.py --dl 22 --ce 7 --budget 100 --reformulation_file=data/genqr_ensemble/dl22.csv```


## Citation

```bibtex
@inproceedings{10.1145/3805712.3809721,
author = {V, Venktesh and Rathee, Mandeep and Anand, Avishek},
title = {When More Reformulations Hurt: Avoiding Drift using Ranker Feedback},
year = {2026},
isbn = {9798400725999},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3805712.3809721},
doi = {10.1145/3805712.3809721},
abstract = {Modern retrieval pipelines increasingly rely on query reformulation and neural reranking to improve effectiveness, but this comes at a significant computational cost and introduces a fundamental tradeoff between recall and query drift. Generating many reformulated queries can substantially increase recall, yet na{\"i}vely merging or exhaustively reranking their results is prohibitively expensive. In this work, we argue that the core challenge is not reformulation generation itself, but the adaptive selection of reformulations and their retrieved documents under a strict inference budget.  We propose ReformIR, a budget-aware retrieval framework that treats query reformulations as first-class features and performs online relevance estimation using a strong reranker as a teacher. Given multiple reformulated queries, ReformIR constructs a large candidate pool and learns a lightweight surrogate model that estimates document utility from reformulation-specific retrieval signals. Under a fixed reranking budget, the surrogate adaptively prioritizes both reformulations and documents, selectively querying a teacher reranker anchored to the original query. This process increases recall while actively suppressing drift through online feature selection over reformulations. We conduct extensive experiments on the MSMARCO passage corpora and TREC Deep Learning benchmarks (DL19–DL22), evaluating effectiveness under realistic reranking budgets. Our results show that ReformIR consistently outperforms existing reformulation strategies, particularly as the number of reformulations increases, where prior methods suffer from severe quality degradation due to drift. Our findings also suggest a shift in retrieval system design: rather than using large language models as rerankers, their capacity is more effectively leveraged in the reformulation stage with feedback-driven optimization. Our code: https://github.com/VenkteshV/ReformIR},
booktitle = {Proceedings of the 49th International ACM SIGIR Conference on Research and Development in Information Retrieval},
pages = {1778–1788},
numpages = {11},
keywords = {query reformulations, reranking, document selection},
location = {Australia},
series = {SIGIR '26}
}
```

## Acknowledgments

This work was partially funded by the Bundesministerium für Wirtschaft und Energie (BMWE), Germany (8ra Initiative "Soofi", 13IPC040E), and by the European Union's Horizon Europe Research and Innovation Programme JustREACH (Grant Agreement No. 101214666). Experiments were supported by NAISS (Swedish Research Council grant 2022-06725) and the Zeus cluster at Stockholm University, DSV.

