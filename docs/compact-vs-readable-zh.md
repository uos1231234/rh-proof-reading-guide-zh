# 从 30 万行到 7.6 万行：黎曼猜想的 Lean 精简版改了什么

先给结论：**终点相同，证明的主线相同，但至少有三处可以具体指出的中间论证变化。** 下文按 Luke Sernau 的 `lsernau/grh-lean` 固定提交 [`72fead10`](https://github.com/lsernau/grh-lean/tree/72fead102f663d5c7a407964a13dc0cf9d977207) 对照 `grh`（可读版）和 `grh-compact`（精简版）。这不是对原始 1,737 万行代码逐行比对，也不是对所有删减的归因。

## 1. 到底缩短了多少？

| 统计口径 | 可读版 `grh` | 精简版 `grh-compact` |
| --- | ---: | ---: |
| 作者 README 的代码行口径 | 约 300,000 | 约 76,000 |
| `src/` 总行数，含空行和注释 | 477,927 | 113,621 |
| `src/` 非空行，含注释 | 432,527 | 95,030 |
| `src/Lib/` 中的 Lean 文件 | 495 | 573 |
| `src/New/` 中的 Lean 文件 | 306 | 219 |

代码行口径约少了四分之三。背景库文件数反而增加，因此不能把缩短理解成简单删除文件；也不能把减少的行数按比例归给下文三个例子。总行数和文件数来自[独立复核的源码统计](https://shosonoda.github.io/lean-rh-alpoge/grh.html)；代码行约数见两版 [README（可读版）](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh/README.md) 和 [README（精简版）](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/README.md)。

## 2. 不变的主线是什么？

两版的 [`Statements.lean`](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/Statements.lean) 内容相同；顶层 [`Main.lean`](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/Main.lean) 仍给出相同的五条定理，包含经典黎曼猜想，以及 Dirichlet L 函数的广义版本。两版都在下面这条链上做文章：

1. **把素数和的偏差接到格上。** 通过反复作 theta 提升，把相关信息写进目标空间中一族向量的估计。theta 提升在这里是从一类自守形式到另一类的变换。
2. **比较同一个量的两边。** A 提供短向量，B 保证有足够多的独立方向，于是能给格的带指数修正的余体积一个上界。C 从整性给下界。底下是许多局部估计；上层要确保左右两边估计的是同一个量。
3. **消掉主项，留下素数偏差。** 两边共同出现的量抵消，给出加权素数和偏差的单边界。再通过解析判据排除临界线右侧的零点，并用函数方程的对称性处理左侧。

阅读入口是精简版 [`New/Main.lean` 的主线说明](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/Main.lean#L26-L72)。可读版对应 [`New/Reduction/MainInequality.lean`](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh/src/New/Reduction/MainInequality.lean)。

## 3. 实例一：第二个截点从“逐点都好”变成“找到一个够好的点”

主不等式同时涉及当前截点和一个更远的截点。要把结论转回当前截点，必须控制远处误差项。但控制所有远处点，与在可用范围里找一个足够好的点，是两个不同的任务。

- **可读版：** 在选定的远截点上，用素数定理分别给相关误差的绝对值上界，再合并。对照 [`OneSidedBound.lean`](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh/src/New/Reduction/OneSidedBound.lean#L95-L124)。
- **精简版：** 通过解析延拓得到长窗口中的存在性：在对数尺度的区间内，存在一个点，使所需的**带符号误差组合**足够小。证明不再要求在这一步分别控制每项误差的绝对值。对照 [`OneSidedCriterion.lean`](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/Endgame/OneSidedCriterion.lean#L73-L114) 及其在 [`Main.lean` 的调用](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/Main.lean#L149-L174)。

这替换的是**选择第二个尺度时的那一步**，不表示精简版不再使用任何素数定理。

## 4. 实例二：局部 Hecke 整除从“沿边逐段控制”变成“先做近似投影”

两版要解决同一类难题：在异常素数处，经过缩放的 Hecke 算子未必把整个目标格都覆盖；要量化还差多少，才能把局部损失计入全局估计。初等因子可理解为：选合适的基以后，看算子在各个独立方向上至少多带了几次该素数。

- **可读版：** 把算子改写成边上函数再汇总，构造近似逆，用 Nakayama 引理控制补足生成所需的额外方向，并沿中间各步合并指数界。[具体分解](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh/src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean#L15-L39)。
- **精简版：** 在泛型 Levi 矩阵的多项式上做共轭不变平均，构造近似投影。这个投影控制哪些方向接近满足 Frobenius 条件，再借助算子两半之间的配对和 Smith 形估计初等因子。[近似投影](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/TwistedHecke/ElementaryDivisors.lean#L4-L17) · [具体构造](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/TwistedHecke/ElementaryDivisors.lean#L150-L225)。

这保留了局部步骤的用途，但不能拿可读版的边函数文件、引理名逐个去精简版里找同名实现。

## 5. 实例三：每个地方不再需要一张适用于所有扭曲的矩阵

先看一个两维例子。在 5 进整数上，第一种条件要求第一坐标能被 5 整除，第二种要求第二坐标能被 5 整除。分别用对角矩阵 `diag(5,1)` 和 `diag(1,5)` 即可，行列式都只含一个因子 5。如果坚持同一张矩阵满足两种条件，它的两行都必须被 5 整除，行列式至少含两个因子 5；`diag(5,5)` 正好达到这个下限。

- **可读版：** 对每个局部嵌入，先找一张矩阵，再要求它对该处所有允许的扭曲都有效。这里的“扭曲”是证明中按参数选取的提升方式；矩阵把给定的一族向量的线性组合送进各扭曲整性条件的交集。[可读版的 `IndexLe`](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh/src/New/Reduction/Lattices/TwistedLattice.lean#L144-L166)。
- **精简版：** 对每个局部嵌入和每个允许的扭曲，可以分别选矩阵。变的是矩阵；各素数的损失预算仍在选矩阵之前统一给出。[精简版的 `Data.IndexLe` 和定理 A](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/Endgame/ShortAndEnough.lean#L128-L141)。
- **为什么够用：** 定理 C 的行列式计算在每个局部嵌入只使用选中的一个扭曲 `θ(ι)`。代码先按这个扭曲取得矩阵，再把它扩到补全后的基上，最终下界仍落在 `V + 2δ` 的形式。[定理 C 的实际调用](https://github.com/lsernau/grh-lean/blob/72fead102f663d5c7a407964a13dc0cf9d977207/grh-compact/src/New/Endgame/LongForms.lean#L34-L65)。

这一步删掉的是对中间结果的一个要求：不必证明同一张矩阵同时处理所有扭曲。上面的 5 进例子只解释为什么量词顺序有区别，不是实际自守形式的算例，也不意味着两版的局部常数或归一化可以直接互换。

## 6. 怎么读，怎么核查？

- 先读精简版的主线说明，找清楚最后比较的量与相消的主项；需要追具体定义和调用时，转到[社区维护版的导读](https://github.com/leanprover-community/riemann-hypothesis/blob/main/GUIDE.md)。社区 `main` 从 Sernau 的**可读版**发展，不是 7.6 万行版。
- [独立复核](https://shosonoda.github.io/lean-rh-alpoge/grh.html)对这个固定提交的两种版本都进行了构建与 Comparator 检查，报告 Lean 和 nanoda 内核接受；五条定理只依赖所列的三个标准公理。这是机器验证，不是对本文每句解释的背书。
- [Andrew Sutherland 的独立复核](https://math.mit.edu/~drew/shortgrh/#what-was-verified)还确认，两版的五个定理陈述展开隐含参数和记号后相同，并分别通过了与挑战文件的对照。源码行数、证明导出大小和核验成本要分开看。精简版的未压缩导出反而大约多 15%，但在报告的同一台机器上，`lean4lean` 的用时和峰值内存从约 2 小时 35 分钟、332 GB 降到约 29 分钟、7 GB。默认 Lean 内核则从约 29 分钟增到约 33 分钟。可读版的 `lean4lean` 约有 2 小时 12 分钟花在 `New.Variety.tiltTheory` 的检查上；精简版重写了这个定义，但这组数据没有隔离出重写中是哪一步造成差异。
- 我们的 [中文导读](../Reading-Pilot-2026-11-26-v2.pdf)第 3 页列出版本对照；正文所引早期实现仍按原提交定位。第 2.4 节的局部保阶算例用于理解一个机制，不能据此声称精简版用了同一个 Type I 构造。
