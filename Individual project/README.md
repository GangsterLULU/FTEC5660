# UNBench (Reproducibility Project for FTEC5660)

UNBench is a comprehensive multi-stage benchmark built on United Nations Security Council (UNSC) records to evaluate large language models across drafting, voting, and statement generation in high-stakes political decision-making.

**Note:** This repository is a modified version of the original UNBench, adapted for the FTEC5660 Reproducibility Project. The evaluation pipeline has been significantly refactored for better security, cross-platform hardware compatibility, and seamless integration with modern LLMs (specifically **Kimi-K2.5**).

## 🚀 Key Modifications in this Fork

To ensure a robust and secure reproducibility evaluation, the following modifications were made to the original codebase:
1. **Model & API Migration:** Replaced the deprecated, hardcoded Together API with Moonshot AI's **Kimi-K2.5** model via `langchain-openai` and the SiliconFlow API.
2. **Zero-Trust Security:** Removed all plaintext API keys from the Jupyter notebooks. Implemented `python-dotenv` for secure local environment variable loading.
3. **Fault Tolerance:** Wrapped the LLM inference loops in `try...except` blocks and added `tqdm` progress bars. This prevents the entire pipeline from crashing due to minor network timeouts.
4. **Cross-Platform Hardware Routing (macOS Support):** Rewrote the hardcoded `device = 'cuda:1'` in Task 4 to dynamically detect hardware (`mps` for Apple Silicon, `cuda` for NVIDIA, or `cpu`), preventing fatal crashes on non-CUDA machines.
5. **SSL Dependency Fix:** Downgraded `urllib3<2` to resolve LibreSSL compilation conflicts commonly found on macOS environments.

---

## 📂 Project Structure

```text
UNBench/
│
├── data/                         # All task datasets (approx. 30 samples/task)
│   ├── task1.json                # Task 1 - Coauthor selection
│   ├── task2.csv                 # Task 2 - Voting simulation
│   ├── task3.json                # Task 3 - Adoption prediction
│   ├── task4.json                # Task 4 - Statement generation
│   ├── task1/ & task2/           # Raw draft files for Tasks 1 & 2
│
├── .env                          # ⚠️ Create this file locally to store your API key
│
├── run_task1(change).ipynb       # Modified Task 1 evaluation pipeline
├── run_task2(change).ipynb       # Modified Task 2 evaluation pipeline
├── run_task3(change).ipynb       # Modified Task 3 evaluation pipeline
├── run_task4(change).ipynb       # Modified Task 4 evaluation pipeline
│
└── README.md                     # Project documentation

🛠️ Installation
1. Clone the repository and navigate to the directory:

Bash
git clone <your_github_repo_url>
cd <your_repo_name>
2. Set up a virtual environment:

Bash
python -m venv .venv
source .venv/bin/activate  # On Windows use: .venv\Scripts\activate
3. Install the required dependencies:

Bash
pip install pandas numpy scikit-learn tqdm imbalanced-learn rouge-score sentence-transformers
pip install langchain-openai python-dotenv
pip install "urllib3<2"  # Fixes NotOpenSSLWarning on macOS
4. Set up API Credentials (Crucial Security Step):
Do NOT hardcode your API key in the notebooks. Instead, create a .env file in the root directory of the project:

Bash
touch .env
Open the .env file and add your SiliconFlow API key:

代码段
SILICONFLOW_API_KEY=your_actual_api_key_here
(Note: The .env file should be added to your .gitignore to prevent accidental commits).

🖥️ Usage & Run Steps
1. Launch Jupyter Notebook:

Bash
jupyter notebook
2. Execute the evaluation pipelines:
Open and "Run All Cells" in the following notebooks sequentially. The tqdm progress bar will show you the real-time execution status.

run_task1(change).ipynb — Coauthor selection.

run_task2(change).ipynb — Voting simulation.

run_task3(change).ipynb — Adoption prediction.

run_task4(change).ipynb — Statement generation (Includes dynamic MPS/CUDA detection for BERT embeddings).

3. Evaluate the outputs:

Tasks 1, 2, & 3 will automatically print classification metrics (Accuracy, AUC, F1 Score, MCC, etc.) at the bottom of the notebooks.

Task 4 will output generation quality metrics (ROUGE-L, Jaccard, and Cosine Similarities).

📖 Original Paper Citation
Benchmarking LLMs for Political Science: A United Nations Perspective Yueqing Liang, Liangwei Yang, Chen Wang, Congying Xia, Rui Meng, Xiongxiao Xu, Haoran Wang, Ali Payani, Kai Shu AAAI 2026 (Oral) 🔗 https://arxiv.org/abs/2502.14122

代码段
@inproceedings{liang2026unbench,
  title={Benchmarking LLMs for Political Science: A United Nations Perspective},
  author={Liang, Yueqing and Yang, Liangwei and Wang, Chen and Xia, Congying and Meng, Rui and Xu, Xiongxiao and Wang, Haoran and Payani, Ali and Shu, Kai},
  booktitle={Proceedings of the AAAI Conference on Artificial Intelligence},
  year={2026}
}