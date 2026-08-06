---
title: "MACCA: Offline Multi-agent Reinforcement Learning with Causal Credit Assignment"
title_zh: MACCA：基于因果信用分配的离线多智能体强化学习
authors: "Ziyan Wang, Yali Du, Yudi Zhang, Meng Fang, Biwei Huang"
date: 2023-09-22
pdf: "https://openreview.net/pdf?id=mFBR2ksIwY"
tags: ["query:mcd"]
score: 9.0
evidence: 基于动态贝叶斯网络的因果信用分配离线多智能体强化学习
tldr: 离线MARL在无法在线交互时很有价值，但因为没有实时反馈和存在复杂交互，准确给每个智能体分配信用非常困难。MACCA将数据生成过程刻画为动态贝叶斯网络，建模环境变量、状态、动作与奖励间的因果关系，并利用离线数据估计该模型。它能为每个智能体学习更精准的信用分配，从而提升离线协作策略的性能。
source: ICLR-2024-Rejected-Public
selection_source: conference_retrieval
motivation: 离线MARL中缺乏实时反馈，现有在线信用分配方法直接迁移效果不佳。
method: 用动态贝叶斯网络建模生成过程，学习状态、动作、奖励间的因果关系以分配信用。
result: 在离线基准上显著改进了信用分配和协作策略性能。
conclusion: 因果建模为离线多智能体信用分配提供了有效解决方案。
---

## Abstract
Offline Multi-agent Reinforcement Learning (MARL) is valuable in scenarios where online interaction is impractical or risky. While independent learning in MARL offers flexibility and scalability, accurately assigning credit to individual agents in offline settings poses challenges due to partial observability and emergent behavior. Directly transferring online credit assignment method to offline settings results in suboptimal outcomes due to the absence of real-time feedback and intricate agent interactions. Our approach, MACCA, characterizing the generative process as a Dynamic Bayesian Network, captures relationships between environmental variables, states, actions, and rewards. Estimating this model on offline data, MACCA can learn each agent's contribution by analyzing the causal relationship of their individual rewards, ensuring accurate and interpretable credit assignment. Additionally, the modularity of our approach allows it to seamlessly integrate with various offline MARL methods. Theoretically, we proved that under the setting of offline dataset, the underlying causal structure and the function for generating the individual rewards of agents are identifiable, which laid the foundation for the correctness of our modeling. Experimentally, we tested MACCA in three environments, including discrete and continuous action settings. The results show that MACCA outperforms SOTA methods and improves performance upon their backbones.

---

## 论文详细总结（自动生成）

## MACCA：基于因果信用分配的离线多智能体强化学习

**作者**：Ziyan Wang, Yali Du, Yudi Zhang, Meng Fang, Biwei Huang  
**会议**：ICLR 2024（投稿，被拒）  
**评分**：9.0

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：离线多智能体强化学习（Offline MARL）在真实环境中在线交互不现实或风险较高（如医疗、自动驾驶等）时具有重要价值。
- **核心难题**：在离线场景下，由于缺乏实时反馈且智能体之间存在复杂交互，准确为每个智能体分配信用（Credit Assignment）非常困难。部分可观测性（Partial Observability）和涌现行为（Emergent Behavior）进一步加剧了这一挑战。
- **已有方法的不足**：直接将在线的信用分配方法迁移到离线设置会导致次优结果，因为离线数据没有实时交互反馈，且智能体间的交互模式更为复杂。
- **论文提出问题的本质**：如何在只有静态离线数据集、没有在线评估反馈的条件下，为每个智能体学习到准确、可解释的信用分配，从而提升离线协作策略的性能。

---

### 2. 论文提出的方法论

- **核心思想**：将数据的生成过程刻画为动态贝叶斯网络（Dynamic Bayesian Network, DBN），通过建模环境变量、状态、动作与奖励之间的因果关系，实现可解释且准确的信用分配。
- **关键技术细节**：
  - **生成式建模**：将离线数据视为一个动态贝叶斯网络的观测样本，该网络显式表征环境动态、智能体动作与个体奖励之间的因果依赖关系。
  - **因果信用分配**：通过分析每个智能体的个体奖励与其他变量之间的因果结构，估计每个智能体对整体团队奖励的实际贡献，以此作为信用分配的准则。
  - **模块化设计**：MACCA 不是一个端到端的全新算法，而是一个可以无缝集成到多种离线 MARL 骨干方法中的模块，即先学习信用分配，再将其作为奖励塑形或价值函数修正项来增强已有算法。
- **公式或算法流程（文字说明）**：
  1. 从离线数据集中估计动态贝叶斯网络的结构（即变量间的因果关系）和参数。
  2. 利用学习到的因果模型推断每个智能体的个体奖励生成函数。
  3. 基于个体奖励到全局奖励的因果贡献，为各智能体生成信用权重。
  4. 将信用权重集成到已有的离线 MARL 骨干算法中，指导策略更新。
- **理论保证**：论文证明在离线数据集设置下，底层因果结构和生成智能体个体奖励的函数是可识别的（Identifiable），为建模的正确性提供了理论基础。

---

### 3. 实验设计

- **实验环境**：共在三个环境中进行测试，涵盖**离散动作**和**连续动作**两种设定。摘要中未给出具体环境名称（如 Multi-Agent Particle Environment、SMAC 等），需查阅论文正文确认。
- **Benchmark 对比**：
  - 对比了 SOTA（State-of-the-Art）离线 MARL 方法。
  - 同时以多个离线 MARL 骨干算法作为 baseline，验证 MACCA 作为模块集成后的性能增益。
- **对比方法**：摘要中未列出具体对比算法名称（如 ICQ、CQL、MADDPG 离线版等），需查阅正文。

---

### 4. 资源与算力

- **未明确说明**：摘要和元数据中未涉及 GPU 型号、数量、训练时长、显存占用等算力信息。
- **结论**：论文在提供的文本范围内未给出具体的计算资源配置细节，需要阅读原论文实验章节获取相关信息。

---

### 5. 实验数量与充分性

- **实验数量**：从摘要来看，进行了三个环境下的实验，涵盖离散与连续动作空间，并对多个 baseline backbone 进行了集成验证。具体实验组数（每个环境下的不同任务、不同骨干组合）在摘要中未展开，需查阅正文。
- **充分性评估**：
  - **优点**：覆盖了离散和连续两类设定，且验证了方法的模块化通用性（集成到多个 backbone 上均有效），实验设计具有一定的广度。
  - **潜在不足**：环境数量偏少（仅三个），未被 SOTA 方法覆盖的环境多样性有限；摘要中未提及消融实验，因此无法评估因果建模、模块化等各设计部件的独立贡献是否被充分验证。客观性和公平性需待原文对比了相同训练数据、相同 backbone、相同超参设置等条件后才能评判。

---

### 6. 论文的主要结论与发现

- MACCA 显著优于现有 SOTA 离线 MARL 方法。
- 在多个离线 MARL 骨干算法基础上集成 MACCA 后，均能稳定提升性能，证明其模块化和通用性。
- 从理论上证明了离线数据下因果结构和个体奖励函数的可辨识性，为该方法的正确性和可靠性提供了支撑。
- 结论：因果建模为离线多智能体信用分配提供了有效且可解释的解决方案。

---

### 7. 优点

- **理论贡献扎实**：不仅提出方法，还给出了可辨识性证明，为因果建模方法在离线 MARL 中的使用提供了理论根基。
- **方法新颖**：将动态贝叶斯网络引入离线 MARL 信用分配问题，超越了传统基于奖励分解或值函数分解的框架，具备更强的因果解释力。
- **模块化设计**：MACCA 可以即插即用地集成到多种离线 MARL 算法中，适用性广，实用性高。
- **问题选择尖锐**：精准切中离线 MARL 中信用分配这一痛点，且有理有据地指出现有在线方法直接迁移的局限性。

---

### 8. 不足与局限

- **实验覆盖有限**：仅三个环境，未能充分展示在复杂真实场景（如大规模智能体、高维视觉输入）下的扩展性；缺少更多元环境（如混合协作-竞争场景）的验证。
- **消融实验可能不足**：摘要中未提及对关键设计选择（如因果模型结构估计的准确性、信用分配粒度的选择等）的消融研究，难以判断各部件贡献。
- **计算开销问题未讨论**：动态贝叶斯网络的结构学习与参数估计在智能体数量多、状态维度高时可能带来显著计算开销，论文摘要未涉及效率问题。
- **偏差风险**：因果结构估计依赖离线数据覆盖质量；若离线数据存在分布偏移或覆盖不全，因果识别的可靠性会受影响，文中未充分讨论这一鲁棒性风险。
- **被拒可能原因推断**：结合评审背景，实验规模有限、baseline 种类不够丰富或理论假设在实际场景中难以满足，可能是导致 ICLR 2024 拒稿的潜在因素之一。

---

（完）
