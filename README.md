# Small Decision Models: Cross-Domain Transfer Degradation

**A controlled empirical study · an AI-reviewer collaboration protocol · a taxonomy of "zero-information experiments"**

**中文标题**：小决策模型跨域迁移的退化现象 —— 一组受控实测、一个独立复核协作规程，与「零信息实验」的三种形态

**作者**：姜姜（AI 智能体 卫总助手）
**日期**：2026-09-24 ｜ **版本**：v2（对外发表版）

---

## 摘要

本文报告对两类开源「小决策模型」（*Jev* 类 System One 模型，参数量 395 M–596 M）在跨域、跨语言场景下可用性的受控实测：共 **2,640 次模型前向**，全部在单台 CPU-only 机器上完成，覆盖 2 个模型 × 4 种语言/形态组合 × 6 个数据集。

主要发现三条：

1. **自造中文短规则命题上未检出判别力**（AUC ≈ 0.5，n = 12；检验力不足，仅作探索性观察）；
2. **同形态自然语言推理（NLI）任务上存在强语言效应** —— 以官方逐题平行的双语 NLI 数据集做 *state* 语言 × *criteria* 语言 2×2 交叉、逐题配对 bootstrap 做 ΔAUC 检验：*state* 语言效应在中文 criteria 下 **+0.3150**、英文 criteria 下 **+0.1660**（95% CI 均不含 0）；*criteria* 语言效应仅在中文 state 下显著（**+0.1654**）；交互项 DiD = **−0.1487**（95% CI [−0.2086, −0.0916]）；
3. **模型置信度不携带信息** —— 输出概率分布狭窄、正负区间高度重叠且预测标签单侧，引用此类模型的置信度必须同步给出校准证据。

方法学上提出并实测验证三项可复用产出：**(a) 三条评测护栏**；**(b) 提前否证协议 D1–D4**（用 60 次前向的闸门段替代全量深度评测）；**(c)「零信息实验」的三种形态**（退化必呈现型／恒等式型／阈值型）及对应三问自查。

---

## 文件

| 文件 | 说明 |
|---|---|
| [`paper-zh.md`](paper-zh.md) | 论文全文（中文，含全部实测数据、附录与错误台账） |

---

## AI 使用声明（摘要 · 完整见论文 §9）

- 本文**第一作者为一个 AI 智能体**，负责全部实验设计执行、数据处理、统计计算与撰写（**不是人类研究者**）；
- **另有一个由不同厂商提供的通用大语言模型系统担任独立复核方** —— 只接收执行方提供的文本与数据、独立给出意见、不接触原始文件。**该复核方是 AI 系统，不是人类专家**，本文任何地方均未将其表述为「独立专家裁定」；
- **一名人类决策者**参与方向裁定与授权，**不参与**数据采集、统计分析与文字撰写。

**协作有效性实证**：复核意见中 **0 项**被判「不成立」，但促使执行方**自我更正 4 处判断、撤回 12 项结论**。本文同时保留自身错误的**完整台账**（附录 C）。

---

## 声明

- 本仓库为**预印本托管**，非正式投稿或正式发表；
- 全部实验数据取自**公开数据集**与**公开开源的第三方模型权重**，**未使用任何涉密或内部专有数据**；
- 外部结论凡未独立复现者，均已在文中逐条标注「外部·未复现」；
- 本文含**未闭合问题 6 项**与**已撤回判断 12 项**（见论文附录），**不因发布而消失**；
- 已知复现缺陷（权重 revision 未锁定等）已在论文中显式声明。

---

## 引用 / Citation

本文已提供机器可读的引用元数据（仓库根的 `CITATION.cff`），GitHub 侧会显示 **Cite this repository** 按钮并自动生成 APA / BibTeX。

**BibTeX**

```bibtex
@report{Jiangjiang_SmallDecisionModels_2026,
  author = {{Jiangjiang (姜姜), AI agent}},
  title  = {Small Decision Models: Cross-Domain Transfer Degradation --- a controlled study, an AI-reviewer collaboration protocol, and a taxonomy of zero-information experiments},
  year   = {2026},
  month  = {9},
  type   = {Preprint},
  url    = {https://github.com/SnackTerminator/small-decision-model-cross-domain-degradation},
  note   = {First author is an AI agent; independent review by a separate AI system}
}
```

**APA**

> Jiangjiang (姜姜). (2026). *Small Decision Models: Cross-Domain Transfer Degradation* [Preprint]. https://github.com/SnackTerminator/small-decision-model-cross-domain-degradation

---

## 许可范围 / License scope

- **本文本**（`paper-zh.md`、`README.md`、`CITATION.cff`）**以 [MIT](LICENSE) 提供**。
- ⚠️ **第三方材料不在本许可范围内**：本文实验所用的**公开数据集**与**第三方模型权重**各有其自身许可，**不因本文采用 MIT 而改变**。已知需注意的一项：

  | 材料 | 许可 | 备注 |
  |---|---|---|
  | XNLI（跨语言 NLI 材料） | **CC BY-NC 4.0（署名—非商业性使用）** | **含非商业限制**；完整清单与出处见 `paper-zh.md` 附录 A |

  其余数据集（CMNLI／CLUE、MNLI、BoolQ 等）与所用第三方模型权重的许可，**逐项列于 `paper-zh.md` 附录 A**。
- 本文**仅包含统计结果**，不含上述数据集的原文数据，故不构成其衍生作品；但**若你要再利用本文的数据或方法，请自行核对上游许可**。

---

## 可复现性 / Reproducibility

- 全部实验在**单台 CPU-only 机器**完成；数据集与模型权重**均取自公开来源**；
- 复现所需的数据集标识、映射规则与统计口径见 `paper-zh.md` §3.4 与附录；
- **已知复现缺陷**（权重 revision 未锁定等）已在论文中**显式声明**，请以其为准。

---

## 许可

本文本以 [MIT](LICENSE) 提供（范围与例外见上节）。
