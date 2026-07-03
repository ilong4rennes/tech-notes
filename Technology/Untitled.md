# 机器学习核心算法与底层逻辑笔记

## 0. 机器学习到底在研究什么

机器学习的基本任务是：

给定数据
$$
[  
\mathcal D={(x_i,y_i)}_{i=1}^{n}  
]
$$
寻找一个函数
$$
[  
f_\theta\mapsto y  
]$$

使它不仅能解释训练数据，还能对没有见过的数据作出较好的预测。

其中：

- (x_i)：输入、特征；
- (y_i)：标签或目标；
- (f_\theta)：模型；
- (\theta)：模型参数；
- (n)：样本数量。

机器学习的核心并不只是“拟合数据”，而是：
$$
[  
\text{从有限样本中学到可泛化规律}  
]
$$
一个完整学习问题通常包含五个部分：
$$
[  
\boxed{  
\text{数据}  
+  
\text{模型}  
+  
\text{损失函数}  
+  
\text{优化算法}  
+  
\text{评估方法}  
}  
]
$$
---

# 第一部分：统一数学框架

## 1. 经验风险最小化

假设模型为 $(f_\theta)$，单个样本上的损失为：
$$
[  
\ell(f_\theta(x_i),y_i)  
]
$$
训练集上的平均损失叫经验风险：
$$
\frac{1}{n}  
\sum_{i=1}^{n}  
\ell(f_\theta(x_i),y_i)  
]
$$
训练通常是在解：
$$
\arg\min_\theta  
\hat R(\theta)  
]
$$
这叫做经验风险最小化，Empirical Risk Minimization，ERM。

真正关心的却是总体风险：
$$
\mathbb E_{(x,y)\sim P_{\text{data}}}  
[  
\ell(f_\theta(x),y)  
]  
]
$$
问题是我们不知道真实数据分布 (P_{\text{data}})，只能使用有限训练样本近似它。

因此机器学习最核心的矛盾是：

> 我们优化的是训练集误差，但真正关心的是未知数据误差。

---

## 2. 正则化

为了防止模型只记住训练数据，常加入正则项：
$$
\arg\min_\theta  
\left[  
\frac{1}{n}  
\sum_i  
\ell(f_\theta(x_i),y_i)  
+  
\lambda \Omega(\theta)  
\right]  
]
$$
其中：

- (\Omega(\theta))：复杂度惩罚；
- (\lambda)：正则化强度。

常见正则化：

### L2 正则化
$$
[  
\Omega(\theta)=|\theta|_2^2  
]
$$
倾向于让参数整体较小。

### L1 正则化

$$[  
\Omega(\theta)=|\theta|_1  
]
$$
倾向于让一部分参数直接变为零，因此具有稀疏性和特征选择效果。

底层逻辑：

> 在多个都能拟合数据的函数中，优先选择更简单、更平滑或参数更受约束的函数。

---

## 3. 偏差—方差分解

对于平方损失，预测误差可以粗略分解为：
$$
\text{Bias}^2  
+  
\text{Variance}  
+  
\text{Noise}  
]
$$
### 偏差 Bias

模型假设太简单，无法表达真实规律。

例如真实关系是非线性的，却使用线性模型。

### 方差 Variance

模型对训练集细节过于敏感。

换一批训练数据，模型结果变化很大。

### 噪声 Noise

数据本身存在不可消除的不确定性。

对应关系：

|状态|训练误差|测试误差|
|---|---|---|
|欠拟合|高|高|
|合理拟合|低|低|
|过拟合|极低|高|

机器学习训练往往是在寻找偏差和方差之间的平衡。

---

## 4. 最大似然估计

很多损失函数并不是随意选择的，而来自概率模型。

假设：
$$
[  
p(y\mid x;\theta)  
]
$$
表示模型认为在输入 (x) 下出现标签 (y) 的概率。

最大似然估计选择：
$$
\arg\max_\theta  
\prod_i p(y_i\mid x_i;\theta)  
]
$$
取对数：
$$
\arg\max_\theta  
\sum_i  
\log p(y_i\mid x_i;\theta)  
]
$$
等价于最小化负对数似然：
$$
\arg\min_\theta  
-\sum_i  
\log p(y_i\mid x_i;\theta)  
]
$$
因此：

- 均方误差对应高斯噪声假设；
- 交叉熵对应分类概率的负对数似然；
- 泊松损失对应泊松分布假设。

---

## 5. MAP 与贝叶斯视角

最大后验估计：
$$
[  
p(\theta\mid \mathcal D)  
\propto  
p(\mathcal D\mid\theta)p(\theta)  
]
$$
选择后验概率最大的参数：
$$
\arg\max_\theta  
p(\mathcal D\mid\theta)p(\theta)  
]
$$
取负对数后：
$$
\log p(\theta)  


$$
这说明正则化可以解释为参数先验：

- L2 正则对应高斯先验；
- L1 正则对应拉普拉斯先验。

频率学派主要寻找一个最优参数。

贝叶斯学派则关注整个后验分布：
$$
[  
p(\theta\mid\mathcal D)  
]
$$
从而表达模型不确定性。

---

# 第二部分：优化基础

## 6. 梯度下降

目标：
$$
[  
\min_\theta L(\theta)  
]
$$
梯度：
$$
[  
\nabla_\theta L  
]
$$
参数更新：
$$
\theta_t

\eta\nabla_\theta L(\theta_t)  
]$$

其中 (\eta) 是学习率。

梯度指向函数上升最快的方向，所以减去梯度是在下降。

---

## 7. Batch、SGD 与 Mini-batch

### Batch Gradient Descent

每次使用全部数据：
$$
\frac1n  
\sum_i  
\nabla \ell_i  
]
$$
优点是梯度稳定，缺点是计算昂贵。

### Stochastic Gradient Descent

每次只使用一个样本：
$$
[  
\theta  
\leftarrow  
\theta-\eta\nabla\ell_i  
]
$$
噪声大，但更新快。

### Mini-batch Gradient Descent

每次使用一小批样本：

\eta  
\frac1B  
\sum_{i\in \mathcal B}  
\nabla \ell_i  
]

这是现代深度学习的标准方式。

Mini-batch 的价值：

- 可以利用 GPU 并行；
- 梯度比单样本稳定；
- 比全量计算便宜；
- 梯度噪声有时有助于逃离差的局部区域。

---

## 8. 常见优化器

### Momentum

\beta v_{t-1}  
+  
\nabla L(\theta_t)  
]

\theta_t-\eta v_t  
]

直觉：积累过去方向，减少震荡。

### RMSProp

维护梯度平方的移动平均：

\beta s_{t-1}  
+  
(1-\beta)g_t^2  
]

更新：

\frac{\eta}{\sqrt{s_t+\epsilon}}g_t  
]

不同参数使用不同有效学习率。

### Adam

同时结合 Momentum 和 RMSProp：

[  
m_t=\beta_1m_{t-1}+(1-\beta_1)g_t  
]

[  
v_t=\beta_2v_{t-1}+(1-\beta_2)g_t^2  
]

Adam 适合快速训练，但不保证在所有任务上泛化最佳。

### AdamW

把权重衰减和梯度更新分离，是 Transformer 和大语言模型中常见的优化器。

---

# 第三部分：监督学习——回归

## 9. 线性回归

模型：

w^\top x+b  
]

损失：

\frac1n  
\sum_i  
(y_i-\hat y_i)^2  
]

矩阵形式：

[  
\hat y=Xw  
]

闭式解：

(X^\top X)^{-1}X^\top y  
]

更稳健的实现通常不会直接计算逆矩阵，而会使用 QR 分解或 SVD。

### 底层假设

线性回归并不一定假设现实关系完全是直线，而是假设：

[  
\mathbb E[y\mid x]  
]

可以由特征的线性组合近似。

### 统计假设

经典线性模型常假设：

[  
y=Xw+\epsilon  
]

其中：

[  
\epsilon\sim\mathcal N(0,\sigma^2)  
]

还常要求：

- 误差均值为零；
- 同方差；
- 样本独立；
- 特征不存在完全多重共线性。

### 优点

- 可解释；
- 训练快；
- 有完善统计推断；
- 适合作为 baseline。

### 失败场景

- 强非线性；
- 大量异常值；
- 特征高度相关；
- 维度远高于样本量且无正则化。

---

## 10. Ridge Regression

目标：

[  
\min_w  
|y-Xw|_2^2  
+  
\lambda|w|_2^2  
]

解：

(X^\top X+\lambda I)^{-1}X^\top y  
]

作用：

- 缓解多重共线性；
- 降低方差；
- 防止参数过大；
- 一般不会把参数精确压到零。

---

## 11. Lasso Regression

目标：

[  
\min_w  
|y-Xw|_2^2  
+  
\lambda|w|_1  
]

L1 在零点不可微，但可用坐标下降、近端梯度等方法优化。

关键性质：

> Lasso 会把部分参数变成零，因此可以做特征选择。

但特征高度相关时，Lasso 可能任意保留其中一个。

---

## 12. Elastic Net

结合 L1 和 L2：

|y-Xw|_2^2  
+  
\lambda_1|w|_1  
+  
\lambda_2|w|_2^2  
]

适合：

- 高维数据；
- 特征相关；
- 希望同时获得稀疏性和稳定性。

---

## 13. 多项式回归

通过构造：

[  
x,x^2,x^3,\dots  
]

将非线性关系转化为线性参数模型：

[  
y=w_0+w_1x+w_2x^2+\cdots  
]

它对参数仍然是线性的。

主要风险是高次多项式容易剧烈震荡和过拟合。

---

## 14. 鲁棒回归

均方误差对异常值非常敏感，因为误差被平方。

### MAE

[  
L=|y-\hat y|  
]

对异常值更鲁棒，但零点不可微。

### Huber Loss

\begin{cases}  
\frac12r^2,& |r|\leq\delta\  
\delta(|r|-\frac12\delta),& |r|>\delta  
\end{cases}  
]

小误差用平方，大误差用线性。

---

# 第四部分：监督学习——分类

## 15. Logistic Regression

虽然名字叫回归，它是分类算法。

二分类模型：
$$
\sigma(w^\top x+b)  
]
$$
其中 Sigmoid：
$$
\frac{1}{1+e^{-z}}  
]
$$
对数几率：
$$
w^\top x+b  
]
$$
损失是二元交叉熵：
$$
-   
    

\sum_i  
[  
y_i\log p_i  
+  
(1-y_i)\log(1-p_i)  
]  
]
$$
### 底层逻辑

Logistic Regression 假设：

> 类别的 log-odds 是输入特征的线性函数。

它的决策边界：

[  
w^\top x+b=0  
]

因此边界是线性的。

### 优点

- 输出概率；
- 可解释；
- 凸优化；
- 训练稳定；
- 很强的基线模型。

### 局限

- 原始特征空间中只能产生线性边界；
- 对复杂交互依赖特征工程。

---

## 16. Softmax Regression

多分类情况：
$$
\frac{  
e^{w_k^\top x}  
}{  
\sum_j e^{w_j^\top x}  
}  
]
$$
交叉熵：
$$
-\sum_i\log p(y_i\mid x_i)  
]
$$
Softmax 把多个分数转换成概率分布。

---

## 17. 朴素贝叶斯

根据贝叶斯公式：
$$
[  
p(y\mid x)  
\propto  
p(y)p(x\mid y)  
]
$$
朴素假设：
$$
\prod_jp(x_j\mid y)  
]
$$
也就是给定类别后，特征条件独立。

虽然这个假设通常不真实，但算法常常有效，尤其在文本分类中。

### 常见类型

- Gaussian Naive Bayes：连续特征；
- Multinomial Naive Bayes：词频；
- Bernoulli Naive Bayes：二值特征。

### 底层优势

高偏差、低方差。

数据较少时，强假设反而能带来稳定性。

---

## 18. K-Nearest Neighbors

预测时寻找离新样本最近的 (k) 个训练样本。

分类：

\operatorname{mode}  
{y_j\in N_k(x)}  
]

回归：

\frac1k  
\sum_{j\in N_k(x)}y_j  
]

### 关键点

KNN 几乎没有训练，主要成本发生在预测阶段。

### 距离度量

欧氏距离：

\sqrt{  
\sum_j(x_j-z_j)^2  
}  
]

高维中会出现“维度灾难”：所有点之间的距离趋于相似。

因此必须：

- 标准化特征；
- 进行降维；
- 选择合理距离；
- 使用近似最近邻索引。

---

## 19. 支持向量机 SVM

线性可分情况下，寻找最大间隔超平面：

[  
w^\top x+b=0  
]

分类约束：

[  
y_i(w^\top x_i+b)\geq1  
]

优化：

[  
\min_{w,b}  
\frac12|w|^2  
]

最大化间隔等价于最小化 (|w|^2)。

软间隔 SVM：

[  
\min  
\frac12|w|^2  
+  
C\sum_i\xi_i  
]

约束：

[  
y_i(w^\top x_i+b)\geq1-\xi_i  
]

对应 Hinge Loss：

\max(0,1-yf(x))  
]

### 支持向量

真正决定决策边界的，主要是靠近边界的样本，即支持向量。

### Kernel Trick

使用核函数：

\phi(x)^\top\phi(z)  
]

不显式计算高维映射 (\phi(x))，却可以在高维特征空间中学习非线性边界。

常见核：

- 线性核；
- 多项式核；
- RBF 高斯核。

RBF：

\exp(-\gamma|x-z|^2)  
]

### 优缺点

适合中小规模、高维数据。

样本量很大时，核 SVM 训练成本高。

---

# 第五部分：树模型

## 20. 决策树

决策树不断选择特征和阈值，把数据切分成更纯的子集。

例如：

[  
x_j<t  
]

分类树常用：

### Entropy

-\sum_kp_k\log p_k  
]

### Information Gain

## H(\text{parent})

\sum_c  
\frac{|S_c|}{|S|}  
H(S_c)  
]

### Gini Impurity

1-\sum_kp_k^2  
]

回归树通常最小化平方误差。

### 底层逻辑

决策树是在输入空间中进行递归分区。

每个叶节点内使用一个简单常数预测。

因此树模型可以理解为分段常数函数。

### 优点

- 可解释；
- 不需要特征缩放；
- 自动建模非线性和特征交互；
- 适合表格数据。

### 缺点

- 单棵树方差很高；
- 很容易过拟合；
- 决策边界不平滑；
- 小数据变化可能导致整棵树变化。

---

## 21. 剪枝

预剪枝：

- 限制最大深度；
- 限制叶节点数量；
- 设置最小样本数；
- 限制最小增益。

后剪枝：

先长出完整树，再删除贡献小的分支。

剪枝本质上是在降低模型复杂度和方差。

---

## 22. Bagging

核心思想：

> 在不同采样数据上训练多个高方差模型，再对结果平均。

Bootstrap：

从训练集中有放回地抽样，得到多个训练子集。

最终预测：

分类：

\operatorname{majority\ vote}  
]

回归：

\frac1M  
\sum_{m=1}^{M}f_m(x)  
]

平均能降低方差，但要求各模型误差不能完全相关。

---

## 23. Random Forest

Random Forest = Bagging + 随机特征子集。

每棵树：

- 使用 Bootstrap 样本；
- 每个节点只从随机一部分特征中寻找最佳切分。

随机选择特征的目的不是降低单棵树误差，而是降低树之间相关性。

### OOB Error

某个样本通常不会出现在所有 Bootstrap 数据集中。

可以用未抽到该样本的树预测它，得到 Out-of-Bag 误差，无需额外验证集。

### 特征重要性

常见方式：

- impurity importance；
- permutation importance。

Impurity importance 容易偏向取值较多的特征。

---

## 24. Boosting

Boosting 的核心是：

> 按顺序训练多个弱学习器，每个新模型专门纠正前面模型的错误。

和 Bagging 的区别：

|   |   |
|---|---|
|Bagging|Boosting|
|并行训练|顺序训练|
|主要降方差|主要降偏差|
|模型相对独立|后一个依赖前一个|
|典型：随机森林|典型：XGBoost|

---

## 25. AdaBoost

维护样本权重。

分类错误的样本权重增加，使下一轮弱分类器更关注它们。

最终模型：

\sum_m\alpha_mh_m(x)  
]

分类：

[  
\hat y=\operatorname{sign}(F(x))  
]

AdaBoost 可以解释为逐步最小化指数损失：

[  
L(y,F)=e^{-yF}  
]

它对异常点比较敏感，因为错误样本会不断获得更高权重。

---

## 26. Gradient Boosting

构建加法模型：

\sum_{m=1}^{M}  
\eta h_m(x)  
]

第 (m) 棵树拟合当前损失关于预测值的负梯度：

-   
    

\frac{\partial L(y_i,F(x_i))}  
{\partial F(x_i)}  
]

对于平方损失，负梯度正好是残差：

[  
r_i=y_i-\hat y_i  
]

所以可以理解为：

> 每棵新树都拟合上一轮尚未解释的误差。

---

## 27. XGBoost

XGBoost 在 Gradient Boosting 基础上加入：

- 二阶泰勒展开；
- 显式正则化；
- 缺失值处理；
- 列采样；
- 行采样；
- 高效并行；
- 树剪枝。

目标近似：

[  
\mathcal L^{(t)}  
\approx  
\sum_i  
[  
g_if_t(x_i)  
+  
\frac12h_if_t(x_i)^2  
]  
+  
\Omega(f_t)  
]

其中：

\frac{\partial \ell_i}{\partial \hat y_i}  
]

\frac{\partial^2\ell_i}{\partial \hat y_i^2}  
]

### 为什么表格数据强

树模型天然适合：

- 非线性；
- 缺失值；
- 类别与数值混合；
- 不同尺度；
- 特征交互；
- 中小规模表格数据。

---

## 28. LightGBM

主要特点：

- Histogram-based splitting；
- Leaf-wise growth；
- 高效处理大规模特征；
- 内存效率较高。

Leaf-wise 会优先分裂收益最大的叶节点，通常收敛快，但小数据上更容易过拟合。

---

## 29. CatBoost

专门增强类别特征处理。

核心包括：

- Ordered Target Statistics；
- Ordered Boosting；
- 减少 target leakage；
- 对类别特征直接编码。

在大量类别变量的表格数据中通常表现出色。

---

# 第六部分：概率生成模型

## 30. 判别模型与生成模型

判别模型学习：

[  
p(y\mid x)  
]

或直接学习决策边界。

例如：

- Logistic Regression；
- SVM；
- 神经网络分类器。

生成模型学习：

[  
p(x,y)  
]

通常分解为：

[  
p(x,y)=p(y)p(x\mid y)  
]

生成模型可以：

- 生成样本；
- 处理缺失数据；
- 建模隐变量；
- 表达数据分布。

---

## 31. Gaussian Discriminant Analysis

假设：

[  
x\mid y=k  
\sim  
\mathcal N(\mu_k,\Sigma_k)  
]

如果不同类别共享协方差矩阵：

[  
\Sigma_k=\Sigma  
]

得到线性决策边界，即 LDA。

如果每类有独立协方差矩阵，则得到 QDA，边界是二次的。

---

## 32. 高斯混合模型 GMM

假设数据由多个高斯分布混合产生：

\sum_{k=1}^{K}  
\pi_k  
\mathcal N(x\mid\mu_k,\Sigma_k)  
]

其中：

- (\pi_k)：混合权重；
- (\mu_k)：均值；
- (\Sigma_k)：协方差。

GMM 是软聚类：每个样本对不同簇有概率归属。

---

## 33. EM 算法

用于含隐变量模型的最大似然估计。

### E-step

根据当前参数计算隐变量后验：

p(z\mid x,\theta^{old})  
]

### M-step

最大化期望完整数据对数似然：

\arg\max_\theta  
\mathbb E_{q(z)}  
[  
\log p(x,z\mid\theta)  
]  
]

EM 不保证找到全局最优，只保证每次迭代不降低似然。

---

## 34. Hidden Markov Model

HMM 用于序列数据。

隐状态：

[  
z_1,z_2,\dots,z_T  
]

观测：

[  
x_1,x_2,\dots,x_T  
]

假设：

p(z_t\mid z_{t-1})  
]

并且：

p(x_t\mid z_t)  
]

核心问题：

- Evaluation：前向算法；
- Decoding：Viterbi；
- Learning：Baum-Welch，即 EM 的一种。

---

# 第七部分：无监督学习

## 35. K-Means

目标：

[  
\min_{{\mu_k}}  
\sum_i  
|x_i-\mu_{c_i}|^2  
]

迭代两步：

1. Assignment：

\arg\min_k  
|x_i-\mu_k|^2  
]

2. Update：

\frac1{|C_k|}  
\sum_{i=k}x_i  
]

### 本质

K-Means 假设簇：

- 大致球形；
- 方差类似；
- 使用欧氏距离；
- 簇中心可以用均值代表。

### 局限

- 需要提前指定 (K)；
- 对初始化敏感；
- 对异常值敏感；
- 不适合非球状簇；
- 不适合不同密度簇。

K-Means++ 改善初始化。

---

## 36. Hierarchical Clustering

### Agglomerative

每个样本开始是一个簇，不断合并。

### Divisive

所有样本开始在一个簇，不断拆分。

簇间距离：

- Single linkage：最近点；
- Complete linkage：最远点；
- Average linkage：平均距离；
- Ward：使簇内方差增长最小。

最终通过树状图 dendrogram 展示层级结构。

---

## 37. DBSCAN

核心参数：

- (\epsilon)：邻域半径；
- minPts：最小邻居数。

样本分为：

- 核心点；
- 边界点；
- 噪声点。

优点：

- 不需指定簇数；
- 能发现任意形状簇；
- 能识别噪声。

缺点：

- 对不同密度簇困难；
- 高维距离退化；
- 参数选择敏感。

---

## 38. PCA

PCA 寻找数据方差最大的方向。

数据中心化后，协方差矩阵：

\frac1nX^\top X  
]

寻找：

[  
\max_{|w|=1}  
w^\top\Sigma w  
]

解是协方差矩阵最大特征值对应的特征向量。

也可以通过 SVD：

[  
X=U\Sigma V^\top  
]

PCA 的两个等价解释：

### 最大方差

保留投影后方差最大的方向。

### 最小重构误差

找到低维子空间，使重构误差最小。

PCA 是线性的，不能表达复杂流形结构。

---

## 39. Kernel PCA

通过核技巧，把数据映射到高维空间后做 PCA。

能学习非线性结构，但扩展到大样本较困难。

---

## 40. t-SNE

主要用于二维或三维可视化。

它尽量保持局部邻域关系，通过最小化两个概率分布之间的 KL 散度实现。

重要警告：

- 全局距离不可靠；
- 簇之间的距离不一定有意义；
- 簇大小不一定有意义；
- 不适合当作通用特征压缩方法；
- 结果对超参数和随机种子敏感。

---

## 41. UMAP

基于流形学习和图结构。

通常比 t-SNE：

- 更快；
- 更能保留部分全局结构；
- 更适合较大数据；
- 可以转换新样本。

但可视化仍然不能被过度解释。

---

## 42. Autoencoder

编码器：

[  
z=f_\theta(x)  
]

解码器：

[  
\hat x=g_\phi(z)  
]

目标：

[  
\min_{\theta,\phi}  
|x-\hat x|^2  
]

通过压缩再重建学习表示。

常见类型：

- Denoising Autoencoder；
- Sparse Autoencoder；
- Variational Autoencoder。

---

## 43. VAE

VAE 学习潜变量生成模型：

p(z)p_\theta(x\mid z)  
]

由于后验：

[  
p(z\mid x)  
]

通常不可直接计算，引入近似后验：

[  
q_\phi(z\mid x)  
]

优化 ELBO：

D_{KL}  
(  
q_\phi(z\mid x)|p(z)  
)  
]

第一项要求重构好。

第二项要求潜空间接近先验，方便采样生成。

---

# 第八部分：神经网络基础

## 44. 多层感知机 MLP

单层：

\sigma(Wx+b)  
]

多层：

\sigma(  
W^{(l)}h^{(l-1)}  
+  
b^{(l)}  
)  
]

输出：

[  
\hat y=f_\theta(x)  
]

如果没有非线性激活，多个线性层相乘仍然等价于一个线性变换。

因此激活函数是神经网络表达非线性的关键。

---

## 45. 激活函数

### Sigmoid

\frac1{1+e^{-x}}  
]

问题：

- 饱和区域梯度接近零；
- 输出非零中心；
- 深层网络容易梯度消失。

### Tanh

\frac{e^x-e^{-x}}{e^x+e^{-x}}  
]

输出零中心，但仍会饱和。

### ReLU

\max(0,x)  
]

优点：

- 计算简单；
- 正区间梯度稳定；
- 产生稀疏激活。

问题：可能出现 dying ReLU。

### GELU

[  
\operatorname{GELU}(x)  
\approx  
x\Phi(x)  
]

Transformer 中常用，比 ReLU 更平滑。

---

## 46. 反向传播

神经网络是复合函数：

L(  
f_L(  
f_{L-1}(  
\cdots f_1(x)  
)  
)  
)  
]

反向传播利用链式法则：

\frac{\partial L}{\partial h_L}  
\cdots  
\frac{\partial h_l}{\partial \theta_l}  
]

反向传播不是优化算法。

它只是高效计算梯度的方法。

真正更新参数的是优化器。

---

## 47. 梯度消失与爆炸

深层网络梯度包含多个 Jacobian 的乘积：

\prod_{k=l+1}^{L}  
\frac{\partial h_k}{\partial h_{k-1}}  
]

如果乘积中的值长期小于 1，梯度消失。

长期大于 1，梯度爆炸。

解决方法：

- 合理初始化；
- ReLU 类激活；
- BatchNorm；
- LayerNorm；
- Residual Connection；
- Gradient Clipping。

---

## 48. 参数初始化

### Xavier Initialization

适合 Tanh：

[  
Var(W)  
\approx  
\frac{2}{n_{in}+n_{out}}  
]

### He Initialization

适合 ReLU：

[  
Var(W)  
\approx  
\frac{2}{n_{in}}  
]

目标是让信号和梯度在各层间保持相似尺度。

---

## 49. Batch Normalization

对 mini-batch 激活标准化：

\frac{x-\mu_B}  
{\sqrt{\sigma_B^2+\epsilon}}  
]

再学习：

[  
y=\gamma\hat x+\beta  
]

作用：

- 稳定训练；
- 允许较大学习率；
- 有轻微正则化效果。

训练与推理时行为不同，推理使用移动均值和方差。

---

## 50. Layer Normalization

沿单个样本的特征维归一化。

不依赖 batch 统计，因此适合：

- Transformer；
- 序列模型；
- 小 batch。

---

## 51. Dropout

训练时随机将激活置零：

m\odot h  
]

其中：

[  
m_j\sim Bernoulli(1-p)  
]

它可以看成训练大量共享参数子网络的近似集成。

现代大型网络中，Dropout 并非永远必要，尤其当数据规模和其他正则化足够时。

---

## 52. Residual Connection

F(x)+x  
]

深层网络不再需要直接学习完整映射，而只需学习残差：

[  
F(x)=H(x)-x  
]

残差连接也为梯度提供短路径，是训练深层网络的关键技术。

---

# 第九部分：卷积神经网络

## 53. CNN

二维卷积：

\sum_{u,v,c}  
W_{u,v,c,k}  
X_{i+u,j+v,c}  
]

CNN 有三个核心归纳偏置：

### 局部连接

邻近像素关系更重要。

### 参数共享

同一个卷积核在整张图片上滑动。

### 平移等变性

输入中的对象移动时，特征响应也相应移动。

这些假设使 CNN 比全连接网络更适合视觉任务。

---

## 54. Pooling

Max Pooling：

\max_{x\in region}x  
]

用于：

- 降低空间尺寸；
- 增大感受野；
- 增强局部平移稳定性。

现代 CNN 有时使用步幅卷积替代池化。

---

## 55. 感受野

某个高层神经元能够“看到”的输入区域。

层数越深，感受野通常越大：

- 浅层检测边缘；
- 中层组合纹理和局部形状；
- 高层表示对象结构。

这不是硬编码，而是训练中逐渐形成的分层表示。

---

# 第十部分：序列模型

## 56. RNN

递归结构：

\tanh(  
W_xx_t  
+  
W_hh_{t-1}  
+  
b  
)  
]

隐藏状态 (h_t) 汇总过去信息。

问题：

- 难以处理长期依赖；
- 梯度消失和爆炸；
- 时间步骤无法充分并行。

---

## 57. LSTM

LSTM 引入门控机制。

遗忘门：

\sigma(  
W_f[x_t,h_{t-1}]  
)  
]

输入门：

\sigma(  
W_i[x_t,h_{t-1}]  
)  
]

候选状态：

\tanh(  
W_c[x_t,h_{t-1}]  
)  
]

细胞状态：

f_t\odot c_{t-1}  
+  
i_t\odot\tilde c_t  
]

输出门：

\sigma(  
W_o[x_t,h_{t-1}]  
)  
]

[  
h_t=o_t\odot\tanh(c_t)  
]

LSTM 的关键是允许信息沿细胞状态较稳定地传播。

---

## 58. GRU

比 LSTM 更简化，只使用更新门和重置门。

参数更少，训练更快，很多任务上效果相近。

---

# 第十一部分：Attention 与 Transformer

## 59. Attention

给定 Query、Key、Value：

[  
Q=XW_Q,\quad  
K=XW_K,\quad  
V=XW_V  
]

Scaled Dot-Product Attention：

softmax  
\left(  
\frac{QK^\top}{\sqrt{d_k}}  
\right)V  
]

底层逻辑：

1. Query 表示“我在找什么”；
2. Key 表示“我包含什么信息”；
3. 点积计算相关性；
4. Softmax 转成权重；
5. 对 Value 加权求和。

---

## 60. Self-Attention

Q、K、V 都来自同一序列。

因此序列中的每个 token 可以直接读取其他 token 的信息。

和 RNN 相比：

- 长距离交互路径短；
- 可以并行处理所有 token；
- 但注意力复杂度通常为：

[  
O(n^2)  
]

---

## 61. Multi-Head Attention

使用多组投影：

Attention(  
QW_i^Q,  
KW_i^K,  
VW_i^V  
)  
]

然后拼接：

Concat(head_1,\dots,head_h)W^O  
]

不同 head 可以学习不同关系：

- 语法；
- 指代；
- 局部模式；
- 长距离依赖。

不能简单认为每个 head 都对应一个人类可解释语义。

---

## 62. Transformer Block

典型结构：

x+  
Attention(LN(x))  
]

x'+MLP(LN(x'))  
]

包含：

- Self-Attention；
- Feed-forward Network；
- Residual Connection；
- LayerNorm。

Feed-forward 通常：

W_2\sigma(W_1x+b_1)+b_2  
]

---

## 63. 位置编码

Self-Attention 本身不理解顺序。

因此需要注入位置信息。

经典正弦编码：

\sin  
\left(  
\frac{pos}{10000^{2i/d}}  
\right)  
]

\cos  
\left(  
\frac{pos}{10000^{2i/d}}  
\right)  
]

现代模型常使用：

- Learned Positional Embedding；
- RoPE；
- ALiBi。

---

## 64. Encoder、Decoder 与 Decoder-only

### Encoder-only

双向注意力。

典型任务：

- 文本分类；
- 表示学习；
- 信息抽取。

代表：BERT。

### Encoder-Decoder

编码输入，解码输出。

典型任务：

- 翻译；
- 摘要；
- 序列转换。

代表：T5。

### Decoder-only

使用因果掩码，只看前面的 token。

目标：

\prod_t  
p(x_t\mid x_{<t})  
]

代表：GPT 类模型。

---

# 第十二部分：表示学习与自监督学习

## 65. 自监督学习

标签由数据本身构造。

例如语言模型：

输入：

[  
x_1,\dots,x_{t-1}  
]

标签：

[  
x_t  
]

不需要人工给每句话标注，但仍然有训练目标。

---

## 66. Word2Vec

### Skip-gram

给定中心词预测上下文词。

目标：

[  
\max  
\sum_t  
\sum_{-c\le j\le c,j\neq0}  
\log p(w_{t+j}\mid w_t)  
]

### CBOW

给定上下文预测中心词。

Word2Vec 的底层思想：

> 出现在相似上下文中的词，会学习到相似表示。

---

## 67. 对比学习

让相似样本表示接近，不相似样本远离。

InfoNCE：

-   
    

\log  
\frac{  
\exp(sim(z_i,z_i^+)/\tau)  
}{  
\sum_j  
\exp(sim(z_i,z_j)/\tau)  
}  
]

常用于：

- 图像表示；
- 多模态对齐；
- 句子表示；
- 自监督预训练。

代表思想：

- SimCLR；
- MoCo；
- CLIP。

---

# 第十三部分：生成模型

## 68. Autoregressive Models

分解联合概率：

\prod_{t=1}^{T}  
p(x_t\mid x_{<t})  
]

优点：

- 精确似然；
- 生成质量高；
- 训练目标清楚。

缺点：

- 生成必须逐步进行，速度慢；
- 错误可能累积。

大语言模型属于自回归生成模型。

---

## 69. GAN

生成器：

[  
G(z)  
]

判别器：

[  
D(x)  
]

目标：

[  
\min_G\max_D  
\mathbb E_{x\sim p_{data}}  
[\log D(x)]  
+  
\mathbb E_{z\sim p(z)}  
[  
\log(1-D(G(z)))  
]  
]

生成器试图欺骗判别器。

判别器试图区分真假。

理论最优时：

[  
p_g=p_{data}  
]

### 主要问题

- 训练不稳定；
- Mode Collapse；
- 对超参数敏感；
- 生成分布覆盖不足。

---

## 70. Diffusion Model

前向过程逐渐给数据加噪：

\mathcal N(  
\sqrt{1-\beta_t}x_{t-1},  
\beta_tI  
)  
]

经过很多步后变成接近高斯噪声。

模型学习反向去噪过程：

[  
p_\theta(x_{t-1}\mid x_t)  
]

常见训练方式是预测噪声：

\mathbb E  
[  
|\epsilon-\epsilon_\theta(x_t,t)|^2  
]  
]

生成时：

```
随机噪声
→ 逐步去噪
→ 样本
```

优点是训练通常比 GAN 稳定，缺点是传统采样过程较慢。

---

# 第十四部分：强化学习

## 71. Markov Decision Process

MDP 包含：

[  
(\mathcal S,\mathcal A,P,R,\gamma)  
]

其中：

- (\mathcal S)：状态；
- (\mathcal A)：动作；
- (P(s'\mid s,a))：状态转移；
- (R(s,a))：奖励；
- (\gamma)：折扣因子。

目标：

[  
\max_\pi  
\mathbb E_\pi  
\left[  
\sum_{t=0}^{\infty}  
\gamma^tr_t  
\right]  
]

---

## 72. Value Function

状态价值：

\mathbb E_\pi  
[  
G_t\mid s_t=s  
]  
]

动作价值：

\mathbb E_\pi  
[  
G_t\mid s_t=s,a_t=a  
]  
]

Bellman 方程：

\mathbb E_\pi  
[  
r+\gamma V^\pi(s')  
]  
]

Bellman 方程把长期问题拆成当前奖励加未来价值。

---

## 73. Q-Learning

更新：

[  
Q(s,a)  
\leftarrow  
Q(s,a)  
+  
\alpha  
[  
r+\gamma\max_{a'}Q(s',a')  
-Q(s,a)  
]  
]

括号中是 TD Error。

Q-Learning 是 off-policy，因为更新目标使用贪心动作，而行为策略可以探索。

---

## 74. DQN

使用神经网络近似：

[  
Q(s,a;\theta)  
]

损失：

Q(s,a;\theta)  
\right]^2  
]

关键技巧：

- Experience Replay；
- Target Network。

---

## 75. Policy Gradient

直接优化参数化策略：

[  
\pi_\theta(a\mid s)  
]

目标梯度：

\mathbb E  
[  
\nabla_\theta  
\log\pi_\theta(a\mid s)  
G_t  
]  
]

REINFORCE 方差较大，因此通常引入 baseline。

---

## 76. Actor-Critic

Actor 学策略：

[  
\pi_\theta(a\mid s)  
]

Critic 学价值函数：

[  
V_\phi(s)  
]

Critic 估计优势：

Q(s,a)-V(s)  
]

Actor 根据优势更新策略。

---

## 77. PPO

PPO 限制策略更新幅度。

概率比：

\frac{  
\pi_\theta(a_t\mid s_t)  
}{  
\pi_{\theta_{old}}(a_t\mid s_t)  
}  
]

Clipped objective：

\mathbb E  
[  
\min(  
r_tA_t,  
clip(r_t,1-\epsilon,1+\epsilon)A_t  
)  
]  
]

核心是避免一次更新把策略推得太远。

---

# 第十五部分：模型评估

## 78. 数据划分

通常：

- Training set：训练参数；
- Validation set：选择模型和超参数；
- Test set：最终无偏评估。

测试集不能反复用于调参，否则它实际上变成了验证集。

---

## 79. 交叉验证

K-fold：

把数据分成 (K) 份，每次使用一份验证，其余训练。

最终平均性能。

适合数据较少时。

时间序列不能随机打乱，应使用时间顺序划分。

---

## 80. 分类指标

混淆矩阵：

|   |   |   |
|---|---|---|
||预测正|预测负|
|实际正|TP|FN|
|实际负|FP|TN|

Accuracy：

[  
\frac{TP+TN}  
{TP+TN+FP+FN}  
]

Precision：

[  
\frac{TP}{TP+FP}  
]

Recall：

[  
\frac{TP}{TP+FN}  
]

F1：

2  
\frac{PR}{P+R}  
]

### 如何选择

- 漏诊代价高：重视 Recall；
- 误报代价高：重视 Precision；
- 类别极不平衡：Accuracy 常误导。

---

## 81. ROC 与 PR Curve

ROC：

- 横轴 FPR；
- 纵轴 TPR。

ROC-AUC 衡量随机正样本排序高于随机负样本的概率。

类别极不平衡时，PR-AUC 通常更有解释力。

---

## 82. 概率校准

一个模型输出 0.8 概率时，理想情况下这类样本中约 80% 应该真实为正。

分类准确率高不等于概率校准好。

常用：

- Reliability Diagram；
- Brier Score；
- Temperature Scaling。

---

## 83. 回归指标

MSE：

[  
\frac1n  
\sum_i(y_i-\hat y_i)^2  
]

RMSE：

[  
\sqrt{MSE}  
]

MAE：

[  
\frac1n  
\sum_i|y_i-\hat y_i|  
]

(R^2)：

1-  
\frac{  
\sum_i(y_i-\hat y_i)^2  
}{  
\sum_i(y_i-\bar y)^2  
}  
]

(R^2) 可以为负，意味着模型不如直接预测均值。

---

# 第十六部分：特征工程与数据问题

## 84. 标准化

\frac{x-\mu}{\sigma}  
]

对以下算法很重要：

- Logistic Regression；
- SVM；
- KNN；
- PCA；
- 神经网络。

树模型通常不敏感。

---

## 85. 类别编码

### One-hot Encoding

每个类别一个二元特征。

适合类别数量较少。

### Ordinal Encoding

适合有自然顺序的类别。

### Target Encoding

使用类别对应目标均值。

必须严格避免数据泄漏，通常应使用 out-of-fold 方式。

---

## 86. 数据泄漏

训练时使用了现实预测时不可获得的信息。

常见形式：

- 在全数据上标准化后再划分；
- 使用未来信息预测过去；
- Target Encoding 使用当前样本标签；
- 同一用户数据同时进入训练和测试；
- 数据增强样本跨集合泄漏。

数据泄漏往往比模型算法选择更危险。

---

## 87. 类别不平衡

处理方式：

- class weight；
- oversampling；
- undersampling；
- SMOTE；
- threshold tuning；
- 合适指标；
- anomaly detection formulation。

不能只机械地重采样，必须考虑真实业务先验和概率校准。

---

## 88. 缺失值

缺失可能是：

- MCAR：完全随机缺失；
- MAR：条件随机缺失；
- MNAR：缺失与自身未观测值相关。

处理方式：

- 删除；
- 均值或中位数填补；
- 模型填补；
- 增加缺失指示变量；
- 使用原生支持缺失的树模型。

缺失本身可能携带信息。

---

# 第十七部分：泛化理论

## 89. 模型容量

模型容量表示模型可以表达多少种函数。

容量太低：欠拟合。

容量太高：理论上可能过拟合。

传统复杂度指标：

- VC Dimension；
- Rademacher Complexity；
- 参数数量；
- 函数范数。

但现代深度学习显示，参数数量并不能完全解释泛化。

---

## 90. PAC Learning

Probably Approximately Correct。

希望以至少 (1-\delta) 的概率学到一个误差不超过 (\epsilon) 的模型。

PAC 理论关注：

- 需要多少样本；
- 假设空间复杂度；
- 学习是否可行。

---

## 91. VC Dimension

衡量分类器能够“打散”多少样本。

线性分类器在 (d) 维空间中的 VC dimension 大致为 (d+1)。

VC dimension 越高，模型表达能力越强，同时需要更多数据约束。

---

## 92. No Free Lunch

没有一个算法在所有可能任务上都最好。

任何算法之所以有效，都是因为它包含某些归纳偏置。

例如：

- 线性模型假设近似线性；
- CNN 假设局部性和平移结构；
- Transformer 假设 token 间可通过注意力交互；
- 树模型假设分段规则有效。

机器学习不是“无假设地从数据发现真理”，而是：

> 用数据在一组预设的函数空间中选择函数。

---

# 第十八部分：模型选择的实用逻辑

## 93. 表格数据

优先 baseline：

- Linear / Logistic Regression；
- Random Forest；
- XGBoost；
- LightGBM；
- CatBoost。

小中型表格数据中，梯度提升树通常非常强。

---

## 94. 图像

常见选择：

- CNN；
- Vision Transformer；
- 预训练模型微调。

数据不多时，优先迁移学习，而不是从零训练。

---

## 95. 文本

传统方法：

- TF-IDF + Logistic Regression；
- Naive Bayes；
- Linear SVM。

现代方法：

- Transformer Encoder；
- Decoder-only LLM；
- Embedding + classifier；
- RAG；
- Fine-tuning。

---

## 96. 时间序列

基础：

- 移动平均；
- Exponential Smoothing；
- ARIMA；
- State Space Models。

机器学习：

- Gradient Boosting + lag features；
- RNN / LSTM；
- Temporal CNN；
- Transformer。

必须特别注意：

- 时间泄漏；
- 分布漂移；
- 滚动验证；
- 季节性；
- 非平稳性。

---

## 97. 图数据

图神经网络利用节点和边。

基本消息传递：

UPDATE  
\left(  
h_v^{(l)},  
AGGREGATE  
{h_u^{(l)}\in N(v)}  
\right)  
]

代表：

- GCN；
- GraphSAGE；
- GAT。

常见问题：

- Oversmoothing；
- Oversquashing；
- 图结构噪声；
- 大图采样。

---

# 第十九部分：研究者必须理解的深层问题

## 98. 优化好不等于泛化好

两个模型都达到接近零训练误差，测试性能可能不同。

原因可能包括：

- 参数范数；
- 解的平坦程度；
- 优化器隐式偏置；
- 数据增强；
- Batch size；
- 模型结构；
- 表示学习质量。

因此研究中要区分：

[  
\text{Optimization Error}  
]

和：

[  
\text{Generalization Error}  
]

---

## 99. 相关不等于因果

大多数监督学习估计：

[  
p(y\mid x)  
]

但因果问题关心：

[  
p(y\mid do(x))  
]

观察到服药者恢复较慢，不代表药物有害，因为严重患者更可能服药。

预测模型可以在分布稳定时表现良好，却不能自动回答干预问题。

---

## 100. Distribution Shift

训练分布：

[  
P_{train}(x,y)  
]

测试分布：

[  
P_{test}(x,y)  
]

可能不同。

类型：

### Covariate Shift

[  
P(x)  
\text{变化，}  
P(y\mid x)  
\text{不变}  
]

### Label Shift

[  
P(y)  
\text{变化，}  
P(x\mid y)  
\text{不变}  
]

### Concept Drift

[  
P(y\mid x)  
\text{变化}  
]

真实生产系统中，分布漂移通常不可避免。

---

## 101. OOD 与不确定性

模型遇到训练分布之外的数据时，可能仍然高置信度输出错误答案。

两类不确定性：

### Aleatoric Uncertainty

数据本身噪声，难以消除。

### Epistemic Uncertainty

模型知识不足，可通过更多数据减少。

常见估计方法：

- Deep Ensembles；
- Bayesian Neural Networks；
- MC Dropout；
- Conformal Prediction。

---

## 102. Conformal Prediction

在较弱分布假设下构建预测集合。

例如分类输出：

[  
C(x)  
\subseteq{1,\dots,K}  
]

希望满足：

[  
P(y\in C(x))  
\geq1-\alpha  
]

它给出的不是单个预测，而是带覆盖率保证的集合或区间。

---

## 103. 可解释性

### 全局解释

模型整体如何决策。

### 局部解释

为什么这个样本得到这个结果。

常见方法：

- Feature Importance；
- Partial Dependence；
- SHAP；
- LIME；
- Saliency Map；
- Counterfactual Explanation。

解释方法本身也可能不稳定，不能把漂亮图表等同于真实因果机制。

---

# 第二十部分：博士阶段应形成的算法分析模板

研究任何算法时，至少回答以下问题。

## 1. Problem

算法解决什么任务？

输入和输出是什么？

## 2. Assumptions

它对数据分布、噪声、独立性和函数形式有什么假设？

## 3. Hypothesis Class

模型能够表达什么函数？

不能表达什么函数？

## 4. Objective

它优化哪个目标函数？

目标函数来自概率模型、几何直觉还是经验设计？

## 5. Optimization

目标是凸还是非凸？

使用闭式解、梯度法、EM、贪心搜索还是组合优化？

## 6. Statistical Properties

估计是否一致？

偏差和方差如何？

样本复杂度是多少？

## 7. Computational Complexity

训练时间、推理时间、内存复杂度分别是多少？

## 8. Failure Modes

哪些数据条件下会系统性失败？

## 9. Evaluation

实验指标是否真的对应研究目标？

Baseline 是否公平？

是否存在数据泄漏？

## 10. Broader Context

它和已有方法相比，真正新增了什么？

改进来自模型、数据、训练预算，还是调参？

---

# 第二十一部分：算法总地图

```
机器学习
│
├── 监督学习
│   ├── 回归
│   │   ├── Linear Regression
│   │   ├── Ridge / Lasso
│   │   ├── Regression Tree
│   │   └── Neural Network
│   │
│   └── 分类
│       ├── Logistic Regression
│       ├── Naive Bayes
│       ├── KNN
│       ├── SVM
│       ├── Decision Tree
│       ├── Random Forest
│       ├── Boosting
│       └── Neural Network
│
├── 无监督学习
│   ├── 聚类
│   │   ├── K-Means
│   │   ├── Hierarchical
│   │   ├── DBSCAN
│   │   └── GMM
│   │
│   ├── 降维
│   │   ├── PCA
│   │   ├── t-SNE
│   │   ├── UMAP
│   │   └── Autoencoder
│   │
│   └── 密度估计
│       ├── KDE
│       ├── GMM
│       └── Generative Models
│
├── 自监督学习
│   ├── Language Modeling
│   ├── Masked Modeling
│   ├── Contrastive Learning
│   └── Representation Learning
│
├── 生成模型
│   ├── Autoregressive
│   ├── VAE
│   ├── GAN
│   └── Diffusion
│
└── 强化学习
    ├── Dynamic Programming
    ├── Q-Learning
    ├── DQN
    ├── Policy Gradient
    ├── Actor-Critic
    └── PPO
```

---

# 最终总纲

几乎所有机器学习算法，都可以用下面这套统一逻辑理解：

[  
\boxed{  
\text{选择一种模型表示}  
}  
]

例如线性函数、树、核函数、神经网络。

[  
\boxed{  
\text{定义什么叫预测得好}  
}  
]

例如平方误差、交叉熵、边际、似然、奖励。

[  
\boxed{  
\text{寻找使目标更优的参数或结构}  
}  
]

例如闭式解、梯度下降、反向传播、树切分、EM、动态规划。

[  
\boxed{  
\text{通过归纳偏置获得泛化}  
}  
]

例如线性、稀疏、局部性、平滑性、参数共享、低维结构。

[  
\boxed{  
\text{在未知数据上验证假设}  
}  
]

使用验证集、测试集、交叉验证、分布外测试和误差分析。

机器学习最底层不是“算法背诵”，而是四个问题：

1. **你假设世界具有什么结构？**
2. **数据给了你什么证据？**
3. **优化过程找到了什么解？**
4. **为什么这个解在新数据上仍然有效？**

博士阶段真正研究的，往往不是“怎样调用一个算法”，而是：

> 某种假设、表示、目标函数和优化方法，在什么条件下成立，在什么条件下失效，以及如何构造更好的学习原则。