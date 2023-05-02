---
title: Jonny
weight: 15
pre: "<b>3. </b>"
chapter: true
---

### Chapter 1

## Year 11

$$
 \lim_{x\to 0} x*sin(\tfrac{1}{x}) 
$$

1. Parametric Functions 参数函数
	- [Definition](https://xaktly.com/ParametricEquations.html)
	- B站video[1分钟搞懂参数方程的原理](https://www.bilibili.com/video/BV1BL4y1F7JQ/?share_source=copy_web&vd_source=e42e20ab1743b1f80efcaa18a191ac13)
	- Wiki: [Parametric equation](https://en.wikipedia.org/wiki/Parametric_equation)
	- [Parametric functions, one parameter](https://www.khanacademy.org/math/multivariable-calculus/thinking-about-multivariable-function/ways-to-represent-multivariable-functions/a/parametric-functions) from Lesson - Visualizing multivariable functions, <Multivariable calculus> at Khan Math website
1. Consequences of the Factor Theorem 
	- Factor Theorem [因式定理 or 因式分解定理](https://baike.baidu.com/item/%E5%9B%A0%E5%BC%8F%E5%AE%9A%E7%90%86/284832), 因式定理是余式定理(Remainder Theorem)的推论，即余式为0时的特殊情况。余式定理, 也叫多项式余数定理, 由由多项式除法的定义导出
	- （多项式）因式定理（Factor theorem）与 余式定理（Polynomial remainder theorem) 多项式长除法(Polynomial long division), [三者关系举例](https://blog.csdn.net/lanchunhui/article/details/77140600)
	- Videos: [Factor Theorem Consequences 1](https://www.youtube.com/watch?v=QXc3vGQG1nk), [Factor Theorem Consequences 2](https://www.youtube.com/watch?v=ILwIlwfIpZQ), [Factor Theorem Consequences 3](https://www.youtube.com/watch?v=pjIeXjm2Fdk)
	- B站video[因式分解高手篇：长除法、试根法、余数定理、因式定理](https://www.bilibili.com/video/BV1Ti4y1M7fJ/?spm_id_from=333.337.search-card.all.click&vd_source=4bb452a07b47a412d37d165e5ff0e9b3)
	- [Khan Math](https://www.khanacademy.org/math/algebra2/x2ec2f6f830c9fb89:poly-div/x2ec2f6f830c9fb89:remainder-theorem/v/polynomial-remainder-theorem-to-test-factor)
	- [A Geogebra game](https://www.geogebra.org/m/xzuGMC8t#material/tmT7PzGu)
1. sums and products zeros
	- [introduction](https://www.cuemath.com/algebra/sum-and-product-of-zeros-in-quadratic-polynomial/)
	- Videos: [Quadratic Function Using Sum and Product of Zeros](https://www.youtube.com/watch?v=BF5VxWUg92o)
	- Videos: [How to find a sum and product of zeros a quadratic equation](https://www.youtube.com/shorts/_X_xXLRavEw)
	- [例题](https://www.varsitytutors.com/precalculus-help/find-the-sum-and-product-of-the-zeros-of-a-polynomial)


#### 组合数学Combinatorics

广义的组合数学（Combinatorics）相当于离散数学，狭义的组合数学是组合计数、图论、代数结构、数理逻辑等的总称。总之，组合数学是一门研究可数或离散对象的科学。
	- B站找到“乐乐课堂” 很多好视频

### 数学分析 

1. 区别：
	1. Analysis分析，把数学当做一门语言来定义，我们最熟悉函数的解析，实数序列和级数的极限论，幂级数和函数论，度量空间或衍生的拓扑向量空间的知识
		- 微积分是分析的衍生学科，也就是分析在几何上的应用部分
	1. 数学分析，则主要是为了把分析和逻辑学，哲学上的分析作区分。即用数学角度建立并使用数学对象，直接分析数学问题并间接分析实际问题，数学分析是20世纪才有的
		- 数学分析假设但你知道有理数（不知道实数），教你如何从有理数构建实数，又证明了极限这个概念在R上是自洽的，于是你可以放心大胆的在R上用极限运算。
		- 微积分是假设你已知道实数，并且告诉你极限运算可以在R上用，于是你就可以calculate啦
1. 微分
	1. 常微分(ODE):单变量one variable
		- 经典：运动是visible（本质是空间位移），一阶导是速度，二阶导是加速度
	1. 多元微积分，or 多变量微积分(Multivariable calculus，multivariate calculus): 偏微分(partial)和全微分(total)
		- Wiki上偏导数和偏微分是同一词条
	1. 微分在牛顿发明的时候很简单，150年后又用极限limit概念“重新发明”了微分
1. 微分方程
	1. 描述某一类函数与其导数之间的关系。微分方程的解是一个符合方程的函数 y=f(x)，初等代数方程里解是常数值
	1. 只有少数简单的微分方程可以求得解析解analytical expression，二次方程的根就是一个解析解的典型例子（初等代数方程里解析解也被称为公式解）
	1. 用数值分析的方法电脑求近似值（数值解）。大多数偏微分方程，尤其是非线性偏微分方程，都只有数值解。
	1. 常微分方程ordinary differential equation只包含一个变量，相对应偏微分方程[partial differential equation](https://zh.m.wikipedia.org/zh-hans/%E5%A4%9A%E5%85%83%E5%BE%AE%E7%A7%AF%E5%88%86)包含多个变量，求偏导数
	1. 和差分方程的理论有密切的关系, 差分方程(difference equation)的坐标只有离散值
		- 差分方程研究递推关系(Recurrence relation）
	1. 经典: population growth model(马尔萨斯模型), logistic模型, Solow模型, 牛顿第二定律
	1. [Wiki](https://zh.wikipedia.org/wiki/%E5%BE%AE%E5%88%86%E6%96%B9%E7%A8%8B)
1. 网课
	1. Coursera平台JHU副教授[Joseph Cutrone](https://www.coursera.org/instructor/josephcutrone)的课程应用性强, level适合悉悉
		- <[Precalculus: Mathematical Modeling](https://www.coursera.org/learn/precalculus-mathematical-modelling)> 
		- <[Applied Calculus with Python](https://www.coursera.org/learn/applied-calculus-with-python)>
	1. Coursera平台Keith Devlin的
		- 高中水平的数学分析课程 <[Introduction to Mathematical Thinking](https://www.coursera.org/learn/mathematical-thinking#syllabus)>, mathematical thinking is a powerful cognitive process. School math typically focuses on learning procedures to solve highly stereotyped problems. Mathematical thinking is a way to solve real problems, problems that can arise from the everyday world, or from science
		- Prerequisite: High school mathematics. Specific requirements are familiarity with elementary symbolic algebra, the concept of a number system (in particular, the characteristics of, and distinctions between, the natural numbers, the integers, the rational numbers, and the real numbers), and some elementary set theory (including inequalities and intervals of the real line). Students whose familiarity with these topics is somewhat rusty typically find that with a little extra effort they can pick up what is required along the way. The only heavy use of these topics is in the (optional) final two weeks of the Extended Course.
		- [斯坦福原网页](https://online.stanford.edu/courses/hstar-y0001-introduction-mathematical-thinking), syllabus: 
			1. Introductory material
			1. Analysis of language – the logical combinators
			1. Analysis of language – implication
			1. Analysis of language – equivalence
			1. Analysis of language – quantifiers
			1. Working with quantifiers
			1. Proofs
			1. Proofs involving quantifiers
			1. Elements of number theory
			1. Beginning real analysis
	1. MIT [Introduction To Analysis](https://ocw.mit.edu/courses/18-100a-introduction-to-analysis-fall-2012/pages/syllabus/)
		- 大学数学系水平
		- Prerequisite: [单变量](https://ocw.mit.edu/courses/18-01sc-single-variable-calculus-fall-2010/pages/syllabus/), [多变量](https://ocw.mit.edu/courses/18-02sc-multivariable-calculus-fall-2010/pages/syllabus/), 微分方程



