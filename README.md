# A Chinese Reading Guide to Divisibility and the Riemann Hypothesis

This repository contains the complete 29-page Chinese pilot chapter of an independent reader's guide to a selected part of the Alpöge-Claude formalization. It helps readers follow the mathematical argument and its Lean references. It is not a full exposition of the proof, a new proof, or a bug report.

## Start here

- [Read or download the current 29-page PDF](Reading-Pilot-2026-11-26-v2.pdf).
- Page 1 introduces the proof strategy. Page 3 maps the original, readable and compact Lean editions. The [earlier 28-page edition](https://github.com/uos1231234/rh-proof-reading-guide-zh/releases/tag/pilot-2026-11-26) is preserved.
- The [previous edition, 23 November 2026](https://github.com/uos1231234/rh-proof-reading-guide-zh/releases/tag/pilot-2026-11-23), is preserved.
- The previous revision added a pencil-and-paper comparison of order in a quotient and at a selected prime ideal (Section 2.4). It illustrates the same-place step; it is not an instance of the original Type I selector.
- The editable LaTeX entry point is `tex/pilot.tex`.

The chapter begins with the example that if 125 and 49 both divide the same nonzero integer, its absolute value is at least 6125. It then separates two roles in the formal argument: divisibility constrains which inputs can belong to a lattice, while matrix estimates control the size and independent directions of the images. These estimates must concern the same maps before they can be combined. The final section examines the constant-True `anyGeometry` field, where the final proof refers to it, and which other hypotheses still carry content.

## Source layout

- `tex/pilot.tex`: main document and reading order.
- `tex/compact-map.tex`: proof outline and cross-edition reading map.
- `tex/opening.tex`: motivation and the opening example.
- `tex/lattice.tex`: divisibility, lattices, and the inputs allowed by the argument.
- `tex/frame.tex`: maps, slopes, and matrix estimates.
- `tex/upstream.tex`: connection to the formal development.
- `tex/geometry-audit.tex`: the `anyGeometry` field and the surrounding hypotheses.
- `tex/closing.tex`: implications, caveats, and reading route.
- `tex/checklist.tex`: reading and recalculation checklist.

## Rebuild the PDF

Install XeLaTeX and the fonts Noto Serif CJK SC and Noto Sans CJK SC. From the `tex/` directory, run XeLaTeX twice:

```sh
cd tex
xelatex pilot.tex
xelatex pilot.tex
```

The current 29-page PDF is in the repository root. Older fixed Releases are unchanged.

## Pinned formal source

References to the original Lean proof are pinned to upstream commit [`40e56bab026552e089d7afcfeff08d2defb9c005`](https://github.com/leanprover-community/riemann-hypothesis/tree/40e56bab026552e089d7afcfeff08d2defb9c005). Section 2.4 separately links to a pinned version of Chey Loveday’s same-place argument. This guide is an independent reader resource.

## Feedback

Open a focused issue in [this repository](<https://github.com/uos1231234/rh-proof-reading-guide-zh/issues>) and include the edition, PDF page or section, the first sentence that became unclear, and evidence that would change the reading. Please consolidate related observations and post at most three contributions per hour and fifteen per day. Draft pull requests with alternative explanations are welcome. Feedback may be in English or Chinese. Independent feedback from human readers and AI agents is welcome; a human endorsement is not required.

---

# 中文导览

本仓库发布一份完整的 29 页中文试读章，是对 Alpöge-Claude 形式化工作中一部分内容的独立读者导读，帮助读者跟上数学论证及其 Lean 代码引用。它不是完整证明的全面讲解、新证明或错误报告。

## 阅读入口

- [阅读或下载新版 29 页 PDF](Reading-Pilot-2026-11-26-v2.pdf)。
- 第 1 页补上证明主线，第 3 页对照原版、可读版和精简版。[此前 28 页版](https://github.com/uos1231234/rh-proof-reading-guide-zh/releases/tag/pilot-2026-11-26)继续保留。
- 保留 [2026 年 11 月 23 日的旧版](https://github.com/uos1231234/rh-proof-reading-guide-zh/releases/tag/pilot-2026-11-23)。
- 上一版在第 2.4 节增加阶 125 的手算对照，说明原商群的阶与指定素理想处的阶为什么可能不同。这个例子只演示保阶原理，不是 Type I 筛选出的原始实例。
- 可编辑的 LaTeX 主文件是 `tex/pilot.tex`。

开篇用一个小例子说明：如果 125 和 49 都整除同一个非零整数，该整数的绝对值至少为 6125。随后将形式化论证中的两项工作分开说明：整除关系限制哪些输入可以进入格，而矩阵估计控制像的大小及独立方向。只有在两项估计针对同一组映射时，才能合并使用。最后一节检查恒为真的 `anyGeometry` 字段、最终证明如何引用它，以及其他假设仍承担什么作用。

## 目录

- `tex/pilot.tex`：主文档与阅读顺序。
- `tex/compact-map.tex`：证明路线与三个源码版本。
- `tex/opening.tex`：动机和开篇例子。
- `tex/lattice.tex`：整除、格，以及论证允许的输入。
- `tex/frame.tex`：映射、斜率和矩阵估计。
- `tex/upstream.tex`：与形式化代码的对应关系。
- `tex/geometry-audit.tex`：`anyGeometry` 字段及相关假设。
- `tex/closing.tex`：影响范围、限定和阅读路线。
- `tex/checklist.tex`：阅读与复算清单。

## 复算 PDF

安装 XeLaTeX 以及 Noto Serif CJK SC、Noto Sans CJK SC 字体。从 `tex/` 目录连续运行两次：

```sh
cd tex
xelatex pilot.tex
xelatex pilot.tex
```

新版 29 页 PDF 位于仓库根目录；旧版固定 Release 不变。

## 固定的形式化源码版本

原证明的 Lean 源码引用固定到上游提交 [`40e56bab026552e089d7afcfeff08d2defb9c005`](https://github.com/leanprover-community/riemann-hypothesis/tree/40e56bab026552e089d7afcfeff08d2defb9c005)。第 2.4 节另引 Chey Loveday 同一位置保阶论证的固定版本。本导读是独立的读者资料。

## 反馈

欢迎在[本仓库](<https://github.com/uos1231234/rh-proof-reading-guide-zh/issues>)针对具体段落开 Issue。请注明版本、PDF 页码或章节、最先让你困惑的句子，以及会改变理解的证据。请合并相关观察，每小时最多三条贡献、每天最多十五条。欢迎通过 Draft PR 提交不同解释。可以使用中文或英文反馈；欢迎人类读者与 AI 智能体独立反馈，无需人类背书。
