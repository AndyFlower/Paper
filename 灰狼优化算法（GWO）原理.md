# 前言：

灰狼优化算法（Grey Wolf Optimizer，GWO）由澳大利亚格里菲斯大学学者 Mirjalili 等人于2014年提出来的一种群智能优化算法。该算法受到了**灰狼捕食猎物活动**的启发而开发的一种优化搜索方法，它具有**较强的收敛性能、参数少、易实现**等特点。近年来受到了学者的广泛关注，它己被成功地应用到了车间调度、参数优化、图像分类等领域中。

# 算法原理：

灰狼隶属于群居生活的犬科动物，且处于食物链的顶层。灰狼严格遵守着一个社会支配等级关系。如图：

![img](images\20190325205936428.png)

**社会等级第一层：**狼群中的头狼记为 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)，![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼主要负责对捕食、栖息、作息时间等活动作出决策。由于其它的狼需要服从![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼的命令，所以 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼也被称为支配狼。另外， ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼不一定是狼群中最强的狼，但就管理能力方面来说， ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼一定是最好的。

**社会等级第二层**：![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 狼，它服从于 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼，并协助 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼作出决策。在 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼去世或衰老后，![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 狼将成为 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼的最候选者。虽然 ![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 狼服从 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 狼，但 ![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 狼可支配其它社会层级上的狼。

**社会等级第三层**：![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 狼，它服从 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha) 、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 狼，同时支配剩余层级的狼。![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 狼一般由幼狼、哨兵狼、狩猎狼、老年狼及护理狼组成。

**社会等级第四层**：![\omega](https://private.codecogs.com/gif.latex?%5Comega) 狼，它通常需要服从其它社会层次上的狼。虽然看上去 ![\omega](https://private.codecogs.com/gif.latex?%5Comega) 狼在狼群中的作用不大，但是如果没有 ![\omega](https://private.codecogs.com/gif.latex?%5Comega) 狼的存在，狼群会出现内部问题如自相残杀。

GWO 优化过程包含了灰狼的**社会等级分层**、**跟踪**、**包围**和**攻击猎物**等步骤，其步骤具体情况如下所示。

**１）社会等级分层**（Social Hierarchy）当设计 GWO 时，首先需构建灰狼社会等级层次模型。计算种群每个个体的适应度，将狼群中适应度最好的三匹灰狼依次标记为 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) ，而剩下的灰狼标记为 ![\omega](https://private.codecogs.com/gif.latex?%5Comega)。也就是说，灰狼群体中的社会等级从高往低排列依次为； ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 及 ![\omega](https://private.codecogs.com/gif.latex?%5Comega)。GWO 的优化过程主要由每代种群中的最好三个解（即  ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) ）来指导完成。

**２）包围猎物**（ Encircling Prey ）灰狼捜索猎物时会逐渐地接近猎物并包围它，该行为的数学模型如下：

![image-20201009114158948](images\image-20201009114158948.png)

式中：t 为当前迭代次数：。表示[ hadamard 乘积](https://blog.csdn.net/dengdengma520/article/details/80539575)操作；A 和 C 是协同系数向量；Xp 表示猎物的位置向量； X(t) 表示当前灰狼的位置向量；在整个迭代过程中 a 由2 线性降到 0； r1 和 r2 是 [0，1] 中的随机向量

**３）狩猎**（ Hunring）

灰狼具有识别潜在猎物（最优解）位置的能力，搜索过程主要靠 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 灰狼的指引来完成。但是很多问题的解空间特征是未知的，灰狼是无法确定猎物（最优解）的精确位置。为了模拟灰狼（候选解）的搜索行为，假设 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 具有较强识别潜在猎物位置的能力。因此，在每次迭代过程中，保留当前种群中的最好三只灰狼（ ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) ），然后根据它们的位置信息来更新其它搜索代理（包括 ![\omega](https://private.codecogs.com/gif.latex?%5Comega)）的位置。该行为的数学模型可表示如下：

![image-20201009114224882](images\image-20201009114224882.png)

式中：![X_{_{\alpha }}](https://private.codecogs.com/gif.latex?X_%7B_%7B%5Calpha%20%7D%7D)、![X_{_{\beta }}](https://private.codecogs.com/gif.latex?X_%7B_%7B%5Cbeta%20%7D%7D)、![X_{_{\delta }}](https://private.codecogs.com/gif.latex?X_%7B_%7B%5Cdelta%20%7D%7D) 分别表示当前种群中 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 的位置向量；Ｘ表示灰狼的位置向量；![D_{_{\alpha }}](https://private.codecogs.com/gif.latex?D_%7B_%7B%5Calpha%20%7D%7D)、![D_{_{\beta }}](https://private.codecogs.com/gif.latex?D_%7B_%7B%5Cbeta%20%7D%7D)、![D_{_{\delta }}](https://private.codecogs.com/gif.latex?D_%7B_%7B%5Cdelta%20%7D%7D) 分别表示当前候选灰狼与最优三条狼之间的距离；当｜A｜＞１时，灰狼之间尽量分散在各区域并搜寻猎物。当｜A｜＜１时，灰狼将集中捜索某个或某些区域的猎物。

![img](images\20190325212837723.png)

从图中可看出，候选解的位置最终落在被 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 定义的随机圆位置内。总的来说， ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 需首先预测出猎物（潜在最优解）的大致位置，然后其它候选狼在当前最优兰只狼的指引下在猎物附近随机地更新它们的位置。

**４）攻击猎物**（Attacking Prey）构建攻击猎物模型的过程中，根据2)中的公式，ａ值的减少会引起 A 的值也随之波动。换句话说，A 是一个在区间［-a，a］**（备注：原作者的第一篇论文里这里是[-2a,2a]，后面论文里纠正为[-a,a]）**上的随机向量，其中ａ在迭代过程中呈线性下降。当 A 在［-1，１］区间上时，则捜索代理（Search Agent）的下一时刻位置可以在当前灰狼与猎物之间的任何位置上。

**５）寻找猎物**（Search for Prey）灰狼主要依赖 ![\alpha](https://private.codecogs.com/gif.latex?%5Calpha)、![\beta](https://private.codecogs.com/gif.latex?%5Cbeta) 、![\delta](https://private.codecogs.com/gif.latex?%5Cdelta) 的信息来寻找猎物。它们开始分散地去搜索猎物位置信息，然后集中起来攻击猎物。对于分散模型的建立，通过｜A｜＞１使其捜索代理远离猎物，这种搜索方式使 GWO 能进行全局搜索。GWO 算法中的另一个搜索系数是Ｃ。从2)中的公式可知，Ｃ向量是在区间范围［０，２］上的随机值构成的向量，此系数为猎物提供了随机权重，以便増加（｜Ｃ｜＞１）或减少（｜Ｃ｜＜１）。这有助于 GWO 在优化过程中展示出随机搜索行为，以避免算法陷入局部最优。值得注意的是，Ｃ并不是线性下降的，Ｃ在迭代过程中是随机值，该系数有利于算法跳出局部，特别是算法在迭代的后期显得尤为重要。