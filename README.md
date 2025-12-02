<div align="center">
<img src="assets/codefuse_logo.png" width="70%">
</div>

## Codefuse-APAR
**Codefuse-APAR** is a state-of-the-art framework for Automated Program Repair (APR) designed to significantly enhance the accuracy and efficiency of software bug fixes through a multi-dimensional, multi-layered strategy. While traditional APR methods are often confined to single-path searches, Codefuse-APAR innovatively combines **Step-wise Optimization** with **Trajectory-wise Verification**, building a dynamic, robust, and scalable repair system.

We believe that high-quality code repair requires not only powerful generation capabilities but also precise judgment and verification mechanisms. Codefuse-APAR is built to solve this core challenge.

> 📦 **Note**: Core code, model weights, and datasets are being prepared and will be open-sourced soon. Stay tuned!

---

## 🚀 Core Features

The core capabilities of Codefuse-APAR are built upon two synergistic strategies:

#### 🧭 Strategy 1: Step-wise Repair - Intelligent Path Exploration

This strategy focuses on how to efficiently explore and sample the most promising repair paths during the patch generation process.

- **🎯 Tool Entropy Dynamic Scheduling**
  - **Challenge**: The APR process involves multiple tools (e.g., compilers, static analyzers, LLMs). How can we balance their performance with the time cost of invocation?
  - **Solution**: By quantifying the uncertainty (entropy) of tool selection, we dynamically assign higher invocation weights to tools with high potential but low utilization. This enables more effective exploration within a limited time budget.

- **🔍 Beam Search for Diverse Sampling**
  - **Challenge**: How can we ensure patch diversity and high quality with a small number of samples (a small K value)?
  - **Solution**: We employ the Beam Search algorithm, which retains the Top-K best candidates at each step instead of choosing a single best path. This allows us to explore far more possibilities than K trajectories with minimal computational cost, significantly increasing the chance of finding the correct fix.
  
  The core idea of Beam Search can be summarized as:
  $$
  \text{Beam}(K) = \underset{Y}{\arg\max} \sum_{y \in Y} \log P(y \mid X)
  $$
  where $X$ is the input (code, error message), $Y$ is the set of retained Top-K candidate sequences, and $P(y \mid X)$ is the probability of a candidate sequence.

- **⚖️ Judge Model for Precise Assessment**
  - **Challenge**: Among multiple generated patches, how can we quickly identify the most likely correct one?
  - **Solution**: We have trained a specialized Judge Model. Using contrastive learning, it learns the deep semantic differences between "good patches" and "bad patches," enabling it to quickly and accurately score patch quality, which serves as a key basis for filtering or re-ranking.

---

#### 🛠️ Strategy 2: Trajectory-wise Repair - Multi-candidate Post-verification

This strategy focuses on how to systematically verify and select the final best patch after multiple repair trajectories have been generated.

- **📊 DeiBase + Selector for Structured Scoring**
  - **DeiBase Scoring**: We use the **unit test pass rate** as the core criterion for scoring. To achieve a more comprehensive evaluation, we introduce a dedicated Agent to generate multi-dimensional unit test cases, and their context is integrated into the DeiBase scoring logic.
  - **Selector Preference Learning**: The Selector is a ranking model trained on human preferences. It first performs an initial scoring of a large pool of candidate patches to recall Top-K. Then, through **data augmentation** and **majority voting**, it further refines the ranking of these Top-K patches to ensure the most suitable fix is chosen.

- **✅ Test Case Verify for Ultimate Validation**
  - **Challenge**: How can we perform the most practical reliability test beyond theoretical evaluation?
  - **Solution**: We extract a complete set of test cases from all repair trajectories. Then, every candidate patch is executed against this **unified, high-coverage test suite**. Ultimately, we select the patch with the **highest number of passed tests** as the final output. This embodies the principle that "practice is the sole criterion for testing truth," greatly enhancing the reliability of the repair.

---




## 🤝 Contributing

We warmly welcome community contributions! Whether it's proposing new ideas, reporting bugs, or submitting pull requests.

Please refer to our [Contributing Guidelines](CONTRIBUTING.md) for details.

---

## 📄 License

This project is licensed under the [Apache License 2.0](LICENSE).

---

## 🙏 Acknowledgments

We thank all the researchers and engineers who have contributed to this project. Special thanks to the Codefuse team for their strong support.

---
<div align="left">
📦 Resources: Code and model weights coming soon.
</div>
