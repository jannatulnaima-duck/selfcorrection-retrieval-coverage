<div id="top"></div>

<br />

<div align="center">

  <h1 align="center">When Self-Correction Fails: Diagnosing Retrieval Coverage in Hallucination Mitigation for Large Language Models</h1>
  <p align="center">
    Joint work between East Delta University and Chittagong University of Engineering &amp; Technology
  </p>

  Trina Chakraborty and [Jannatul Naima](mailto:jannatulnaima.cse.cu@gmail.com).
</div>

<!-- TABLE OF CONTENTS -->

<details open>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#reproduce-results">Reproduce results</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#references">References</a></li>
  </ol>
</details>


<!-- ABOUT THE PROJECT -->

## About The Project

This repository contains the code for our study of claim-level self-correction for hallucination mitigation. The framework splits a generated answer into atomic claims, retrieves evidence for each claim, verifies every claim with a natural language inference model, and revises only the claims that fail, committing a revision only when it raises the aggregate support score. We compare it with a vanilla LLM and a retrieval-augmented baseline on TruthfulQA, HaluEval and FEVER, and diagnose why it helps as a hallucination detector but hurts as a generator: the retrieval coverage is too low for claims to be verified.

:star2: Official implementation to reproduce the main experiments of our paper "When Self-Correction Fails: Diagnosing Retrieval Coverage in Hallucination Mitigation for Large Language Models".

The notebook has two parts:

* **Part I**: the self-correcting framework, the baselines, ablations, significance tests and the retrieval diagnostics.
* **Part II**: an adaptive extension (an `UNVERIFIABLE` verdict, risk-conditioned routing and adaptive retrieval depth) motivated by the diagnosis in Part I.

**If you use our code, please consider citing:**

    @inproceedings{chakraborty2026selfcorrection,
      title={When Self-Correction Fails: Diagnosing Retrieval Coverage in Hallucination Mitigation for Large Language Models},
      author={Chakraborty, Trina and Naima, Jannatul},
      booktitle={11th IEEE Asia-Pacific Conference on Computer Science and Data Engineering (CSDE)},
      year={2026}
    }

<!-- GETTING STARTED -->

## Getting Started

### Prerequisites

* Python 3.10 or higher
* pip version 21 or higher
* A CUDA GPU (we used an NVIDIA A100)
* About 25 GB of free disk space for the model weights and the retrieval corpus

### Installation

1. Clone the repo

   ```sh
   git clone https://github.com/jannatulnaima-duck/selfcorrection-retrieval-coverage.git && cd selfcorrection-retrieval-coverage
   ```

2. Create and activate an environment

   ```sh
   python -m venv .venv
   source .venv/bin/activate
   ```

3. Install required libraries

   ```sh
   pip install -r requirements.txt
   ```

<!-- USAGE EXAMPLES -->

## Reproduce results

The notebook was written for Google Colab, so the easiest way to run it is to upload `notebooks/selfcorrect_adaptive_colab.ipynb` to Colab with a GPU runtime. Run the cells from top to bottom.

### Part I: self-correcting framework

* The configuration cell holds every setting (models, number of examples, retrieval and threshold parameters, seed).
* The next cell writes the helper module `selfcorrect_lib.py` to disk, and the following cells build the retrieval corpus and the FAISS index.
* The two thresholds are calibrated on a held-out HaluEval split that does not overlap with any test split.
* The notebook then runs the vanilla LLM, the RAG baseline and the self-correcting system on the test splits, runs the ablations, and computes the metrics, the paired significance tests, the tables, the figures and the diagnostic analysis.

### Part II: adaptive extension

Run Part I up to the calibration cell first. Part II writes `adaptive_lib.py`, fits a risk model on held-out HaluEval items, and runs the adaptive system and its ablation arms. Its evaluation cells are the last cells of the notebook.

* Generator: `Qwen/Qwen2.5-7B-Instruct` (4-bit), verifier: `microsoft/deberta-large-mnli`, embeddings: `sentence-transformers/all-mpnet-base-v2`.
* The outputs of our run are kept in the notebook.

<!-- LICENSE -->

## License

Distributed under the MIT License.


<!-- CONTACT -->

## Contact

Jannatul Naima - jannatulnaima.cse.cu@gmail.com & Trina Chakraborty - trinasust5002@gmail.com.
Contact us via email or create github issues if you have any questions/requests. Thanks!

<!-- ACKNOWLEDGMENTS -->

## References

* Stephanie Lin, Jacob Hilton, and Owain Evans. 2022. TruthfulQA: Measuring How Models Mimic Human Falsehoods. In Proceedings of ACL 2022.
* Junyi Li, Xiaoxue Cheng, Wayne Xin Zhao, Jian-Yun Nie, and Ji-Rong Wen. 2023. HaluEval: A Large-Scale Hallucination Evaluation Benchmark for Large Language Models. In Proceedings of EMNLP 2023.
* James Thorne, Andreas Vlachos, Christos Christodoulopoulos, and Arpit Mittal. 2018. FEVER: A Large-Scale Dataset for Fact Extraction and VERification. In Proceedings of NAACL-HLT 2018.
* Sewon Min, Kalpesh Krishna, Xinxi Lyu, et al. 2023. FActScore: Fine-grained Atomic Evaluation of Factual Precision in Long Form Text Generation. In Proceedings of EMNLP 2023.
* Pengcheng He, Xiaodong Liu, Jianfeng Gao, and Weizhu Chen. 2021. DeBERTa: Decoding-enhanced BERT with Disentangled Attention. In ICLR 2021.
* Nils Reimers and Iryna Gurevych. 2019. Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks. In Proceedings of EMNLP-IJCNLP 2019.
* Jeff Johnson, Matthijs Douze, and Herve Jegou. 2019. Billion-scale similarity search with GPUs. IEEE Transactions on Big Data 7(3), 535-547.

<p align="right">&#40;<a href="#top">back to top</a>&#41;</p>
