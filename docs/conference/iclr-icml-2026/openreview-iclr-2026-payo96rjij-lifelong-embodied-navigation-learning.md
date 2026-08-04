---
title: Lifelong Embodied Navigation Learning
title_zh: 终身具身导航学习
authors: "Xudong Wang, Jiahua Dong, Baichen Liu, Qi Lyu, Lianqing Liu, Zhi Han"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=PaYo96rjij"
tags: ["query:maspd"]
score: 4.0
evidence: 终身具身导航学习，并非多智能体协调
tldr: 具身导航智能体在持续学习新导航任务时容易灾难性遗忘旧知识。本文形式化终身具身导航学习问题，提出Uni-Walker框架，将导航知识解耦为任务共享与任务特定组件，使用解码器扩展LoRA、知识继承与专家协同激活策略。该框架在多种场景和用户指令风格下持续适应并保持先前知识，为终身导航智能体的可持续发展提供了方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 具身导航智能体在持续多场景学习时存在灾难性遗忘问题。
method: 将导航知识解耦为共享与特定部分，结合DE-LoRA、知识继承与专家协同激活。
result: 在多样场景与指令风格下持续适应并保持已有导航知识。
conclusion: 知识解耦与参数扩展能有效缓解终身导航中的灾难性遗忘。
---

## Abstract
Embodied navigation agents powered by large language models have shown strong performance on individual tasks but struggle to continually acquire new navigation skills, which suffer from catastrophic forgetting. We formalize this challenge as lifelong embodied navigation learning (LENL), where an agent is required to adapt to a sequence of navigation tasks spanning multiple scenes and diverse user instruction styles, while retaining previously learned knowledge. To tackle this problem, we propose Uni-Walker, a lifelong embodied navigation framework that decouples navigation knowledge into task-shared and task-specific components with Decoder Extension LoRA (DE-LoRA). To learn the shared knowledge, we design a knowledge inheritance strategy and an experts co-activation strategy to facilitate shared knowledge transfer and refinement across multiple navigation tasks. To learn the specific knowledge, we propose an expert subspace orthogonality constraint together and a navigation-specific chain-of-thought reasoning mechanism to capture specific knowledge and enhance instruction-style understanding. Extensive experiments demonstrate the superiority of Uni-Walker for building universal embodied navigation agents with lifelong learning. We also provide the code of this work in the Supplementary Materials.

---

## 论文详细总结（自动生成）

# 论文总结：终身具身导航学习（Lifelong Embodied Navigation Learning）

> **注意**：本文档仅基于论文摘要与元数据撰写，论文正文的详细内容未在给定材料中完整呈现，因此部分内容（如实验细节、数据集名称、算力配置等）无法核实，凡涉及推断之处均已明确标注。

---

## 1. 论文的核心问题与整体含义（研究动机与背景）

- **研究背景**：具身导航智能体（如基于大语言模型构建的导航系统）在单任务场景下已展现出较强性能，但其学习范式是“一次训练、一次部署”，无法应对现实世界中持续出现的新场景、新指令。
- **核心问题**：当导航智能体需要按顺序学习多个任务（跨场景、跨指令风格）时，会遭遇**灾难性遗忘（catastrophic forgetting）**——学习新技能的同时会丢失旧知识，导致导航能力不可持续。
- **问题形式化**：论文首次将这一挑战系统性地定义为**终身具身导航学习（Lifelong Embodied Navigation Learning, LENL）**，要求智能体在连续学习一系列导航任务的过程中，既能适应新场景与新指令风格，又能保留既有知识和能力。
- **整体含义**：该研究试图回答“如何让导航智能体在开放、动态环境中具备可持续学习能力”，为通用具身智能体的长期部署提供理论框架与技术路径。

---

## 2. 论文提出的方法论

### 2.1 核心思想：导航知识解耦
- **知识分离**：将导航知识拆分为两类——
  - **任务共享知识（task-shared）**：跨任务通用的导航能力（如避障、朝向估计、语义理解等）；
  - **任务特定知识（task-specific）**：针对某一场景或某种指令风格独有的导航策略。
- 这种解耦设计使得新任务的学习可以**复用共享知识**，同时仅需要更新少量特定参数，从而缓解旧知识的遗忘。

### 2.2 关键技术组件

| 组件 | 作用 |
|---|---|
| **Decoder Extension LoRA（DE-LoRA）** | 在LLM解码器上扩展低秩适配模块，以参数高效的方式为新任务开辟独立的知识子空间，避免干扰已有参数 |
| **知识继承策略（Knowledge Inheritance）** | 将先前任务学到的共享知识传递给新任务，作为初始化的先验，加速新任务收敛并保持旧知识连续性 |
| **专家协同激活策略（Experts Co-activation）** | 在推理/训练时动态激活多个任务专家，协同完成当前导航决策，强化共享知识的跨任务迁移与迭代精炼 |
| **专家子空间正交性约束（Expert Subspace Orthogonality Constraint）** | 约束不同任务特定专家的参数子空间相互正交，减少任务之间相互干扰，防止灾难性遗忘 |
| **导航特定链式思维推理（Navigation-specific Chain-of-Thought）** | 为导航任务设计专门的CoT推理机制，分步理解用户指令并映射为具体导航行为，增强对多样化指令风格的适应能力 |

### 2.3 算法流程（文字描述）
1. **任务序列输入**：智能体按顺序接收多个导航任务，每个任务包含不同的场景与指令风格；
2. **知识初始化**：首个任务训练后，提取共享知识；后续任务通过知识继承策略获得先验；
3. **参数扩展**：每个新任务通过DE-LoRA扩展解码器参数，并使用正交性约束隔离任务特定子空间；
4. **协同学习**：训练时通过专家协同激活策略联调共享知识与特定知识；
5. **持续推理**：推理阶段利用导航特定CoT机制理解指令，综合所有专家知识生成导航行动序列。

---

## 3. 实验设计

- **数据集**：论文摘要中指出实验覆盖“多种场景（multiple scenes）和多种用户指令风格（diverse user instruction styles）”，但**未在摘要中给出具体数据集名称**（如Habitat、Matterport3D、VLN-CE等）——由于正文未提供，无法确认。
- **Benchmark**：论文提出的是一个新问题（LENL），因此其benchmark应为**自建的连续学习导航基准**，包含任务序列划分和遗忘度评估协议——但具体设定细节需查阅正文。
- **对比方法**：摘要仅提及“Extensive experiments demonstrate the superiority of Uni-Walker”，**未列出具体对比的基线方法**。可合理推测对比对象包括：
  - 传统微调方法（Fine-tuning）
  - 经典持续学习方法（如EWC、LwF、SI等）
  - 其他参数高效微调方法（如Adapter、Prompt-based方法）

---

## 4. 资源与算力

- **信息缺失**：论文摘要与元数据中**完全没有提及**任何算力相关细节（如GPU型号与数量、训练时长、参数量、显存占用等）。
- **说明**：这一信息在完整论文中可能出现在实验设置部分，但当前可获取材料不足以确认。

---

## 5. 实验数量与充分性

- **已呈现信息**：摘要仅声称进行了“广泛实验”，但没有列出实验组数。
- **推测**：基于方法论复杂度，完整论文可能包含以下实验类别：
  - 不同任务序列长度下的性能比较；
  - 与多类持续学习基线的对比；
  - DE-LoRA、知识继承、协同激活、正交性约束、CoT推理等组件的消融实验；
  - 遗忘度（Backward Transfer）与迁移增益（Forward Transfer）的量化分析。
- **充分性评估**：
  - 由于信息不足，**无法判断实验的统计充分性**（如重复次数、方差分析等）；
  - 从正反面看：多个技术组件均有独立设计动机，如果逐一做了消融则充分性较好；但缺少数据集细节和对比方法清单，**公平性与完整性需要在正文中核实**。

---

## 6. 论文的主要结论与发现

- **核心结论**：通过知识解耦（共享 + 特定）+ 参数扩展（DE-LoRA）+ 知识继承与协同激活 + 正交性约束，能够**有效缓解终身导航学习中的灾难性遗忘**。
- **能力提升**：Uni-Walker能够在持续接触新场景和新型指令风格后，仍保持对旧任务的导航能力，实现“学新不忘旧”的持续适应。
- **通用性**：论文声称该框架为构建**通用的终身具身导航智能体**提供了可行路径。

---

## 7. 优点（方法或实验设计的亮点）

- **问题定义新颖**：首次将“终身学习”与“具身导航”结合并形式化（LENL），填补了该交叉领域的问题定义空白。
- **方法设计系统性强**：没有简单套用通用持续学习技巧，而是针对导航任务特点定制了完整解决方案——
  - 知识解耦逻辑清晰（共享 vs 特定）；
  - 参数高效（LoRA）适合大规模LLM部署；
  - 正交性约束理论上能严格限制任务干扰；
  - 导航专用CoT将指令理解与行为生成衔接起来。
- **实际意义强**：面向机器人长期部署的真实需求，有助于未来服务机器人在复杂家庭/户外环境中不断成长。
- **提供了代码**：论文在补充材料中附带了代码，便于复现和后续研究。

---

## 8. 不足与局限

- **实验信息不透明（当前材料）**：
  - 未列出具体数据集、场景数量、指令风格类型；
  - 未给出对比方法完整列表；
  - 未提供算力配置。
  - 这些问题导致**无法从现有材料全面评估实验的充分性与公平性**。
- **技术局限性（推断）**：
  - 专家扩展策略会随任务数量增长带来**参数规模线性增长**，长期运行的资源开销可能偏高；
  - 正交性约束与知识继承之间存在潜在冲突（继承要求重叠，正交要求分离），需要精巧平衡；
  - 任务的顺序敏感性（task ordering effect）是否被系统性分析不清楚；
  - 仅在仿真场景验证（若如此），**仿真到现实的泛化差距**未讨论。
- **应用限制**：真实导航环境噪声大、指令不确定性高，当前框架在sim-to-real迁移上的表现未知。
- **对比公平性**：如果没有与任务级增量学习、无示例持续学习（class-incremental vs task-incremental）等设定进行严格区分，结果的可比性有限。

---

## 总结

该论文针对具身导航智能体的灾难性遗忘问题，提出了一个完整的终身学习框架Uni-Walker，通过导航知识解耦、DE-LoRA参数扩展、知识继承、专家协同激活与导航链式推理等技术，在跨场景、跨指令风格的连续学习任务中取得了优于现有方法的性能。其问题定义和方法系统性是显著贡献，但**由于当前材料中缺乏实验数据集与算力细节，整体实验的客观性、充分性和公平性尚需阅读全文后进一步评估**。

（完）
