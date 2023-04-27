---
title: Dad notes
weight: 50
---


**寻找数学应用课程**

- Coursera搜："math physics", "python physics", "python scientific" 
	- 昨天和梁老师聊了11-12数学学习，让我进一步思考了这个阶段的数学学习，应该是“纯数学”和“应用”共同进行了，换句话说，就是数学分析和python scientific programming要同步进行，抽象世界和真实世界（两条腿）一起进步，不止是单纯的纸面数学，因为超常儿童对真实世界的具体触摸本身较少，所以更应该加强python编程这个交互的窗口
	- 上个千年的教育基础是“阅读、写作和算术”；现在是阅读、写作和计算。学习编程是每个学生教育的重要组成部分 The basis for education in the last millennium was “reading, writing, and arithmetic;” now it is reading, writing, and computing. Learning to program is an essential part of the education of every student
- Google搜："real analysis python", "mathematical analysis python", 

**数学系1-3年级**

分析类：

- 数学分析(1)：实数理论、极限、单变量微积分
- 数学分析(2)：多变量微积分、曲面上的积分
- 数学分析(3)：级数理论、傅立叶分析
- 实分析：测度论和Lebesgue积分
- 复分析(1)：最基本的复分析
- 泛函分析：最基本的泛函分析
- 常微分方程：存在性、唯一性、延拓定理
- 偏微分方程(1-2)：波方程、热方程、泊松方程的存在唯一性, 椭圆方程、双曲方程、抛物方程的存在唯一正则性
- 分析力学：Lagrange力学以及一些玄学
- 概率论：最基本的概率论

代数类：

- 线性代数：矩阵与行列式, 矩阵的对角化
- 代数学前沿基础：模论、范畴论、同调代数
- 抽象代数(1)：基本的群环域 (学了抽象代数，相当于打开了代数类的大门)
- 代数数论(1)：赋值理论，素数定理
- 李群李代数：复半单李代数的表示论

几何类：

- 微分流形：流形的概念以及流形上常见的研究对象
- 拓扑学：点集拓扑、基本群、复叠空间、同调理论 (学了拓扑学和微分流形，相当于打开了几何类的大门)

![数系统5层](https://raw.githubusercontent.com/ctang83/NB_img/main/numbersys.png){width=35%}

![自然数-整数-有理数-实数-复数](https://raw.githubusercontent.com/ctang83/NB_img/main/自然数整数有理数实数复数.png){width=90%}

名词解释

- Category theory 范畴论：以抽象的方法来处理数学概念，将这些概念形式化成一组组的“对象”及“态射”
- Set theory 集合论: 数理逻辑I只涉及一阶逻辑这个完备体系下的问题，尽管关系是用集合的方式表示，但是并不依赖集合论的知识。而数理逻辑II的才会讨论更一般的问题，其第一，也是最主要的部分就是集合论。所以可以把学习路线简化为： 数理逻辑的完备部分 -> 集合论 -> 数理逻辑的不完备部分
	- 集合, 就是把一堆东西放在一起, 之间的互相作用我们称之为操作或者运算
	- **群 = 集合 + 运算**
- Group theory 群论：刻画结构的一种非常基础的工具。应用: 魔方，正交表，高次方程解析解，密码学，拓扑基本群
- Measure theory 测度论：测度论是实分析的基础，就好比实数理论之于数学分析。传统的积分是在区间上进行的，后来人们希望把积分推广到任意集合上，就发展出测度的概念。
	- 实变函数的核心是勒贝格积分，而勒贝格积分是黎曼积分的推广。黎曼积分的核心思想是分割积分区间然后求黎曼和取极限，如极限存在则称函数为黎曼可积的。但是黎曼积分有很大的局限性，他对函数的性态要求很高，要求函数几乎处处连续。
	- 勒贝格积分的思想跟黎曼积分不同，他不分割函数的积分区间，而是分割函数的值域区间，然后乘以对应积分区间的测度求和取极限。测度论是用来度量积分区间的，输入一个积分区间，测度函数给出一个正的实数。对于一维空间来说，测度就是区间的长度，对于二维区间来说，测度表示积分区域的面积，对于三维空间，测度表示积分区间的体积，对更高维的空间来说，测度代表类似体积的东西。可见测度论是实变函数的核心理论。
	- 测度论是*概率论*的理论基础
- Order theory 序理论
- Graph theory 图论：
- Complex analysis 复分析：the branch of mathematical analysis that investigates functions of complex numbers 复变函数
	- 实分析中的泰勒级数和傅里叶级数, 这两者都是关于某个函数的级数展开式, 两者的系数分别是通过求导和积分得出来的, 实数世界里两者毫不相关, 复分析却告诉我们：它们是同一个东西！只是将其在不同的角度“投影”到实数世界里，就产生了不同的“物像”。所以**复数揭示了微分与积分的一种隐藏的联系--即全纯函数微分与积分等价**
- Real analysis 实分析：实数的数学分析
- Functional analysis 泛函分析：
- Vector calculus 向量分析，或向量微积分，“向量分析”有时也用作多元微积分的代名词，包括向量分析，偏微分和多重积分等多元实分析

![Map of Math](https://raw.githubusercontent.com/ctang83/NB_img/main/mapmath.jpg){width=35%}

**怎样学微积分**

AP Calculus AB course is typically equivalent to one semester of college calculus:

- Analysis of graphs (predicting and explaining behavior)
- Limits of functions (one and two sided)
- Asymptotic and unbounded behavior
- Continuity
- Derivatives: At a point, As a function, Higher Order derivatives, Techniques
- Integrals: Properties, Techniques, Numerical approximations
- Fundamental theorem of calculus
- Anti-differentiation
- L'Hôpital's rule 洛必达法则

AP Calculus BC further includes the following:

- Convergence tests for series
- Taylor series
- The use of parametric equations
- Polar functions (including arc length in polar coordinates)
- Calculating curve length in parametric and function equations
- Integration by parts
- Improper integrals
- Differential equations for logistic growth
- Using partial fractions to integrate rational functions

**Application of Calculus** 

First Order Differential Equations

- Separable Equations
- Homogeneous Equations
- Linear Differential Equations of First Order
- Exact Differential Equations
- Using an Integrating Factor
- Bernoulli Equation
- Riccati Equation
- Implicit Differential Equations
- Singular Solutions of Differential Equations
- Lagrange and Clairaut Equations
- Differential Equations of Plane Curves
- Orthogonal Trajectories
- Radioactive Decay
- Barometric Formula
- Rocket Motion
- Newton’s Law of Cooling
- Fluid Flow from a Vessel
- Population Growth
- Learning Curve
- Advertising Awareness

Second Order Differential Equations

- Reduction of Order
- Second Order Linear Homogeneous Differential Equations with Constant Coefficients
- Second Order Linear Nonhomogeneous Differential Equations with Constant Coefficients
- Second Order Euler Equation
- Bessel Differential Equation
- Chebyshev Differential Equation
- Curvature of Plane Curves
- Equation of Catenary
- Newton’s Second Law of Motion
- Newton’s Law of Universal Gravitation
- Mechanical Oscillations
- Nonlinear Pendulum
- Oscillations in Electrical Circuits

Higher Order Differential Equations

- Cases of Reduction of Order
- Equations Solvable in Quadratures
- Differential Operators
- Higher Order Linear Homogeneous Differential Equations with Constant Coefficients
- Higher Order Linear Nonhomogeneous Differential Equations with Constant Coefficients
- Higher Order Linear Homogeneous Differential Equations with Variable Coefficients
- Higher Order Linear Nonhomogeneous Differential Equations with Variable Coefficients
- Higher Order Euler Equation
- Beam Deflection

Systems of Differential Equations

- Linear Homogeneous Systems of Differential Equations with Constant Coefficients
- Method of Eigenvalues and Eigenvectors
- Construction of the General Solution of a System of Equations Using the Method of Undetermined Coefficients
- Construction of the General Solution of a System of Equations Using the Jordan Form
- Method of Matrix Exponential
- Linear Nonhomogeneous Systems of Differential Equations with Constant Coefficients
- Linear Systems of Differential Equations with Variable Coefficients
- Basic Concepts of Stability Theory
- Equilibrium Points of Linear Autonomous Systems
- Stability in the First Approximation
- Method of Lyapunov Functions
- Routh-Hurwitz Criterion
- First Integrals
- Mass-Spring System
- Double Pendulum
- Home Heating

## Oct 2022 - Jan 2023

**Seeking for help**

1. USyd: [David Easdown](http://www.maths.usyd.edu.au/u/de/) de@maths.usyd.edu.au
	- Jonny finished his Calculus on Coursera
	- He has interests in the interface between secondary and tertiary education
	- His course [MATH3066: Algebra and Logic](https://www.sydney.edu.au/units/MATH3066) fit Jonny
1. 寻求帮助邮件- 阐述Jonny's case
	1. 给James Ruse
		- Irene Chork, the relieving Head Teacher of Mathematics at James Ruse Agricultural High School. I just wanted to update you on organising some enrichment for your son in Mathematics. We have Maths Enrichment and Olympiad classes before/after school with smaller class sizes. They begin around mid to late Term 1 and Jonny might enjoy these problem-solving mathematical challenges.
		- Reply: he is currently self-studying an online-course named "Introduction to Calculus" provided by The University of Sydney on the Coursera platform. 
		- Sarah Powell, contact EPS Principal for Jonny's math learning support. 
	1. 给[Department of Edu](hpge@det.nsw.edu.au)，specific enquiries related to High Potential and Gifted Education
	1. 给大学\数学系\GERRIC邮件 
		- [UNSW](office.MathsStats@unsw.edu.au) and [Talented Students Program, Professor Jie Du](j.du@unsw.edu.au)
		- [USyd School Office](maths.schooloffice@sydney.edu.au) general enquiries; General academic advice enquiries regarding Mathematics should be directed to [Dr Alexander Fish](advisor_degree_math@maths.usyd.edu.au)
		- [MQ, Associate Professor Steve Lack, Director of Education,](steve.lack@mq.edu.au)

## April - July, 2022

1. Google "high school math support"
	1. Google "Mathematics Enrichment Club"
	1. Google "sydney math enrichment"
		- [James Ruse Agricultural High School](https://jamesruse-h.schools.nsw.gov.au/learning-at-our-school/mathematics.html)The study of mathematics
		- Mathematical Association of NSW 提供 [Student Activities](https://www.mansw.nsw.edu.au/student-activities/competitions) 里4个方面支持, 尤其是student support和competition. 
		- [National Mathematics Summer school](https://nmss.edu.au/about/)澳洲国家数学暑假学校, 每年8月报名, 次年1月两周培训, 完成Year 11 Extension 1的学生报名
		- [AMC Maths Enrichment](https://www.amt.edu.au/enrichment)报名参加, 我看了里面的stage excerpts, 悉悉水平在Dirichlet (years 6–7)和Gauss (years 8–9)
		- [Mega Maths Day](https://www.sydney.edu.au/science/industry-and-community/community-engagement/mega-maths-day.html)悉尼大学2022年11月活动注册
		- [Meet a Mathematician](https://www.sydney.edu.au/science/industry-and-community/community-engagement/meet-a-mathematician.html)悉尼大学1h meeting发邮件
		- [UNSW Mathematics Enrichment Club](https://www.unsw.edu.au/science/our-schools/maths/engage-with-us/high-school-students-and-teachers/unsw-mathematics-enrichment-club): students from years 8 to 12. The club runs as an extra-curricular event at several Sydney schools, but everyone can join in by working through the weekly problem sheets. 
		- [支持高成就和有兴趣的学生 5 至 18 岁](https://nrich.maths.org/7737): 剑桥大学数学学院的支持资源
1. Google "youngest HSC student"
	- 2014, 11-year-old 
1. Google "mathematical geography"
	1. Google "geography numeracy worksheets": 
		- [US Census统计局Geography Worksheets](https://www.census.gov/programs-surveys/sis/activities/geography.html)
		- [英国地理协会 numeracy and geography](https://www.geography.org.uk/Curriculum-planning/Numeracy-and-geography)
	1. Google "Mathematics and Geography Travel"
1. google "sydney primary school gifted math extension"
	1. GERRIC program at UNSW provides [a list of resources](https://www.unsw.edu.au/arts-design-architecture/our-schools/education/professional-learning/gerric-gifted-education/resources)
	1. GERRIC suggest [Gifted and talented services](https://www.unsw.edu.au/content/dam/images/photos/campus/kensington/2021-06-ada-documents/2021-06-gifted_and_talented_directory_2018.pdf)
	1. why some gifted programs prefer extension approach rather than acceleration approach (跳级)? 
		- Gifted programs are often just an extension of existing studies. We wanted to expand on what was already on offer and bring in students from different domains of learning: academic, creative and athletic so we can cater for the learning needs of all high-potential students.

**悉悉rocket scientist**

1. The [Beginner's Guide to Rockets](https://www.grc.nasa.gov/www/k-12/rocket/guided.htm) was created as a Web-based textbook
1. corrective feedback loop - Elon Musk

**MIT undergraduate course课程**

1. [LINEAR ALGEBRA](https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/)This is a basic subject on matrix theory and linear algebra. Emphasis is given to topics that will be useful in other disciplines, including systems of equations, vector spaces, determinants, eigenvalues, similarity, and positive definite matrices.
1. [6.0001 Introduction to Computer Science and Programming in Python](https://ocw.mit.edu/courses/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/) is intended for students with little or no programming experience. It aims to provide students with an understanding of the role computation can play in solving problems and to help

**programs for applications**

1. St Ives北公立小学[The Ku-ring-gai Unit for Gifted and Talented Students](http://web1.stivesnth-p.schools.nsw.edu.au/stivesnth-p/s/G%26T_Home.html) provides full-time educational enrichment for identified gifted students in Years 3, 4, 5 and 6. Our long-standing expertise in gifted education began in 1991 and we continue to advocate for providing differentiated programs for gifted students. 北区最有名的项目, 也叫天才班
	- 这个新闻报道[他们怎么上课学习](https://www.smh.com.au/education/sydney-school-getting-students-into-top-high-schools-without-tutoring-20180823-p4zzbf.html), 如11 岁的 Lauren Korenblyum 就读于 St Ives North 的 5 年级，她说她喜欢天才班学生可以完成的课外活动和充实活动。辛克莱女士说：“我们有医生、工程师、飞行员、书法家、数学家、机器人技术人员，这远远超出了课程范围。”
1. St Aloysius' College悉尼北区海边提供 [Gifted and Talented](https://www.staloysius.nsw.edu.au/college-life/academic/learning-enrichment)
	- 发邮件问transfer 

**学障资优生Twice exceptional**

1. [2e学障资优生wiki](https://zh.m.wikipedia.org/zh-hans/%E5%AD%A6%E9%9A%9C%E8%B5%84%E4%BC%98%E7%94%9F)
1. [2e孩子有一系列独特的问题需要解决](https://childmind.org/article/twice-exceptional-kids-both-gifted-and-challenged/)

天赋异禀的儿童，同时也有学习障碍，他们是教育和育儿的独特挑战，很多人不习惯一个人既有天赋，又有残疾的想法。2e面临的最常见的问题之一就是没有被认识到，这意味着他们的独特需求得不到解决。

一个孩子可能是一个非常有才华的读者，但他或她可能缺乏数学和逻辑能力，或者一个孩子可能会谱写交响乐，但不能写出自己的名字。有些残疾实际上与天赋倾向有关，比如阿斯伯格综合症、诵读困难，有学习障碍但又有超常天赋的儿童被称为2e儿童可以以各种方式呈现在某些情况下，孩子的天赋可以弥补学习障碍，本质上是隐藏了学习障碍。在这种情况下，一个2e孩子可能会觉得学习很容易，甚至很无聊，直到他遇到一个主要的障碍，在这种情况下，学习障碍表现出来。这可能会严重损害孩子，因为他可能得不到早期的帮助来解决学习障碍，孩子也可能会与困难作斗争，学习放弃那些太难的东西，因为其他事情都太容易了。

另一种情况下，人们可能过于关注学习障碍，以致于错过了2e孩子的天赋。这种情况经常发生在读写困难的儿童身上。诵读困难者可能会被分流到另一个教育轨道上，而这又不允许学生发展其他技能，因此阅读障碍被视为这是一个需要解决的关键性障碍。许多2e孩子都在与成就和失败作斗争，而一个表现不如预期的孩子可能正与学习障碍作斗争。例如，一个孩子表现出非凡的阅读和写作能力，但口头理解能力差，这可能是孩子有听力障碍的迹象，或者他或她无法集中精力于口头呈现的材料上。

一个2e孩子也可能对太容易的材料感到厌烦或坐立不安，从而导致跌倒教育者和家长们开始认识到2e儿童和他们独特的需求因为这些孩子经常不接受用来对孩子进行分类的测试，所以这类孩子的父母支持他们的孩子以确保他们得到他们需要的教育是很重要的。与老师讨论孩子的情况是一个很好的开始，它还可以帮助利用医生、心理学家和其他人的服务能够解决学习障碍的专业人士，同时为孩子的天赋而庆祝。Anyway 许多患有阿斯伯格综合症的儿童和青少年都非常聪明。

[video](https://www.youtube.com/watch?v=fnphw3kME8k)

![ee](https://raw.githubusercontent.com/ctang83/NB_img/main/amyjonny/ee1.png){width=40%}

![ee](https://raw.githubusercontent.com/ctang83/NB_img/main/amyjonny/ee2.png){width=40%}

![ee](https://raw.githubusercontent.com/ctang83/NB_img/main/amyjonny/ee3.png){width=40%}


## Jan - March, 2022

### NSW新州数学教学体系3

1. 在Gumtree上找数学书, 想着虽然从图书馆找书对比, 但高中毕竟不一样, 很多内容以后要反复练习和回顾, 悉悉在家里用的时间长, 还是买一套好了
1. 然后发现, 新州NSW和维州VIC 西澳WA等对Stage 6几条线的数学书称谓又不一样, 他们叫Mathematics Essential, Applications, Methods, Specialist, 几个层级和NSW也吻合, 我觉得适用描述非常清晰, 摘录如下:
	1. Mathematics **Specialist** is the only ATAR mathematics course that should not be taken as a stand-alone course and it is recommended to be studied in conjunction with the **Methods** ATAR course as preparation for entry to specialised university courses such as engineering, physical sciences and mathematics.
	1. Mathematics **Methods** focuses on the use of calculus and statistical analysis. The study of calculus provides a basis for understanding rates of change in the physical world, and includes the use of functions, their derivatives and integrals, in modelling physical processes. The study of statistics develops students’ ability to describe and analyse phenomena that involve uncertainty and variation.
	1. Mathematics **Applications** focuses on financial modelling, geometric and trigonometric analysis, graphical and network analysis, growth, decay in sequences, statistical questions that involve univariate, bivariate data, time series data. **Applications** is designed for students who want to extend their mathematical skills beyond Year 10 level, but whose future studies or employment pathways do not require knowledge of calculus, including continuing their studies at university or TAFE.
1. Gumtree上搜year 11 math extension, 大部分卖的textbook都是两套: CambridgeMATHS 和 Maths in focus
1. CambridgeMATHS这套应该是王牌, 1)在parramatta图书馆同一本书数量最大, 2)本书作者群5个有4个是Sydney Grammar School的数学老师, 同时和HSC有联系, 换句话说NSW高考数学出题圈hidden network可能和SGS有关 

再根据我第一篇的选书 Extension 1 Year 11 -> Extension 1 Year 12 -> Extension 2 Year 12, 来列一下CambridgeMATHS系列的内容:

| Extension 1 Year 11    |   Extension 1 Year 12   |   Extension 2 Year 12   |
|:----------------------:|:-----------------------:|:-----------------------:|
|1 Methods in alegbra    | 1 Sequences and series  | 1 Complex Numbers I |
|2 Numbers and surds     | 2 Mathematical induction – Extension 1 | 2 Proof |
|3 Functions and graphs  | 3 Graphs and equations | 3 Vectors |
|4 Transformations and symmetry | 4 Curve-sketching using the derivative | 4 Complex Numbers II |
|5 Further graphs        | 5 Integration | 5 Integration |
|6 Trigonometry          | 6 The exponential and logarithmic functions | 6 Mechanics |
|7 The coordinate plane  | 7 The trigonometric functions |
|8 Exponential and logarithmic functions | 8 Vectors – Extension 1 |
|9 Differentiation       | 9 Motion and rates |
|10 Polynomials          | 10 Projectile motion – Extension 1 |
|11 Extending calculus   | 11 Trigonometric equations – Extension 1 |
|12 Probability          | 12 Further calculus – Extension 1 |
|13 Discrete probability distributions | 13 Differential equations – Extension 1 |
|14 Combinatorics        | 14 Series and finance |
|15 The binomial expansion of Pascal's triangle | 15 Displaying and interpreting data |
|16 Further rates        | 16 Continuous probability distributions |
|17 Further trigonometry | 17 The binomial distribution – Extension 1 | 

NSW官网介绍高中数学即Stage 6的话, 最简洁直观的[页面](https://educationstandards.nsw.edu.au/wps/portal/nesa/11-12/stage-6-learning-areas/stage-6-mathematics)


### NSW新州数学教学体系2

具体课本(study and teaching / textbook)的挑选, 是个很麻烦的事, 因为到了Stage 6, 面临澳洲这边的高考

这边有高中文凭HSC(Higher School Certificate), 也有类似高考的叫ATAR, 当然只有想读大学的人参加(挺多人不参加的, 因为毕竟是才开拓了二百年的"前殖民地", 荒野多人又少, 可供人类提供体力活改造的"世界"也挺多, 脑力活的"内卷"程度就要低很多)

`THE Universities Admissions Centre (UAC) has announced that the Australian Tertiary Admission Rank (ATAR) for 2021 NSW HSC students will now be released at 9am on Thursday 20 January 2022.`

`The Higher School Certificate (HSC) is the highest educational award in New South Wales schools. It is awarded to NSW students who have satisfactorily completed Years 11 and 12 at secondary school.`

但是, 澳洲的高考和大学录取呢, 又不完全重合, 尤其现在海外大学生下降的背景下, 很多大学开始跳过ATAR直接通过Year 11的成绩就录取(锁定)学生, 而且还私下给学费减免.

话说回来, 我们比较了下面四个系列的书, 图书馆都有借:

1. Maths in focus -- 最早在Ryde图书馆看到
1. CambridgeMaths stage 6 
1. Oxford insight Mathematics general
1. Cambridge Mathematics 3 Unit (Extension 1) -- 这套直接放弃, 编排密度高, 练习过多, 例题过程不够

![Cambridge_mathematics_3U_extension_1](https://raw.githubusercontent.com/ctang83/NB_img/main/amyjonny/fig_cambridgemathematics3U.png){width=40%}

![CambridgeMaths](https://raw.githubusercontent.com/ctang83/NB_img/main/amyjonny/fig_camMaths.png){width=40%}

![Oxford_insight](https://raw.githubusercontent.com/ctang83/NB_img/main/amyjonny/fig_oxfordmath.png){width=40%}

 
### NSW新州数学教学体系1

悉悉2022年3月6日, 正式学完K-Year10的数学, 已自学内容包括: 

1. 上海数学小学阶段
1. 公文数学 Kumon math A to K-level, 
1. NAPLAN Year 7, 
1. IB Mathematics Grade 8/9, 
1. Cambridge MATHS NSW Year 9/10 

前面三个是以前学习的, 到悉尼后发现NSW有一套数学体系, 为了适应这边的内容开始进入NSW的教材和workbook. 然后计划进入下一阶段, 发现其复杂性有点超过我们的想象, 随后做了一点研究, 先整理点名词:

1. K11-12, 也对应 Year 11 和 Year 12, 都叫Stage 6, 对应IB体系的IBDP阶段(IB和IBDP请自查)
1. 但不同的州对 Year 11-12 定位不一样, ACT首领地和TAS叫"大学前阶段", 其他地方还是归为中学, 可比作中国的高中一二年级(我和老婆开始以为是高三读两年, 后来觉得还是高一高二更合适, 这边还有个Year 13, 类比高三, 但不完全围绕高考)
1. 2012年的NSW教学改革后, 数学分4个难易级别: 标准, 高级, extension 1(3Unit), extension 2(4Unit). 
1. 最低的标准=life skills级别, 我和Summer觉得和Year 10内容差不多; 
1. 高级advanced数学内容明显就加深了, 就出现微积分了, 我翻了一下他们94年的所谓"高考试卷", 这种级别的卷子已经非常难了
    1. 有时候看到叫enhanced的教科书, 这个叫法应该是改革前的
	1. 包含关系: Extension 1 Year 11(12) course includes Advanced Year 11(12) course
	1. 包含关系: Extension 2 Year 12 course includes Extension 1 Year 12 course & Advanced Year 12 course

选书结论: Extension 1 Year 11 -> Extension 1 Year 12(900多页) -> Extension 2 Year 12 (书和课程不一样)

我和Summer觉得高中生学数学按照学术能力划分非常合理, 类似中国大学生考研究生选择 数一, 数二, 数三 的卷子

但现在我们的问题是: 悉悉怎么自学后面的数学呢? 

1. 数学4条路线, 第2条advanced的书看过, 不难. 总之第1和第2条线和前面Year10相比, 扩展不大
1. 第3条线的教科书我对比了, 是cover第2条线内容的(800多页 VS. 500多页), 
1. 第4条线的书比较薄, 明显是第3条的递进, 结论就是选第3条线(extension 1 还有个preliminary配合)


### LMS学习管理系统

今天在搜索gifted scholarship时, 走到塔州教育部网页-gifted children support, 发现他们推荐Canvas也就是一种online learning management system, 我好奇走进去看了一下

发现这是一个很大的project, 要把人的整个教育经验打包塞进一个数据集里, 包括K-12和Higher Edu的所有学习,练习,测试等所有环节

Canvas的竞争对手是Google Classroom, 他们的区别在于其发展路径: 
`Canvas is primarily used by higher education institutions, while Google Classroom is primarily used by primary education institutions`

在用户评价上, Google比Canvas在价格,界面等方面要好


### Sydney Grammar School悉尼文法学校申请

PS：2023年1月记录，后来我们才知道SG是个纯人文学校，是培养“讲故事的人”的地方，不是给“做数学”的人准备的学校

悉悉的申请悉尼文法学校其实已经有了进展, 我和Summer准备的材料内容应该是很impressive, 但是写的statement真的语病很多(我回头去看真羞愧), 但是不管怎样收到了reply说5月底Year 3 assessment, 下个月会通知details, 今天先挖个坑, 贴一些新足迹帖子讨论里的摘录:

```r
-- 考试内容与流程
三年级招一个班，大约是15-18人——参加考试的大约100-150。
2018 y3 entrance exam 
9:00 AM 参加 year3入学考试，100多个孩子吧。
快九点是校长简单介绍几句就让大家放松，就让孩子们上楼，家长们也慢慢散了。

中午12点20左右，只见孩子们一批批被放了出来。考了什么：
1）reading comprehension，貌似有三四篇文章，之后有选择题和true or false
2）creative writing，20分钟写一篇文章，儿子说时间够
3）maths和problem solving
两套题，problem solving 较难，儿子出来就说最后一道题肯定错了。没有除法，分数有一道。
看来还是没有超大纲，考基本功和灵活运用
4）spelling，20个单词，长的有photograph，儿子说没学校难

按姓氏顺序分班，和其他班的家长交流下来，感觉不止一套试题，可能是为了保密性考虑吧

2017 y3 entrance exam 
英语: 考词汇量考知识面：这次是填空题，有难度哦...
例举一下：Dolphins manage to keep an Eye Out While Sleeping in order to look out for____.
小A填predators（掠食者），我感觉填danger可能更好！不一定只有捕食者，也可能要应对各种危机嘛。
阅读：阅读短篇，是非题。
听力和写作：听老师讲一个故事，然后接下去续写故事，所以既考了听力，又考写作能力。总之是看孩子英语全方面的能力，很有挑战性！
数学: 小A说是35道选择题，时间不够，没有锻炼过所以答题速度跟不上，不过据说题目不是特别难。

-- St Ives Sydney Grammar Y3考试
今年因为是疫情关系，所以以往的一轮笔试，一轮小组面试，压缩到一天完成。从早上9点到下午1点。
孩子们集中在一起准备出发，引领他们的老师问大家有没有问题。

考试内容。
第一部分是25道数学题，比较简单，GA占了较大比重，45分钟内完成。
(儿子说他15分钟就做完了，然后去看书。他很高兴找到了H.G Well 的The Invisible Man，看了两个chapter）
第二部分是阅读理解，好像就几道题，也比较简单，也是30/45分钟左右完成。(具体他也说不出来了， 反正就是不难)
第三部分是写作，老师读了一个（寓言）故事，然后孩子要按照这个故事写自己的寓言故事。

小组活动是拼图，500片那种大拼图。
儿子抱怨说拼图很难，他只找了7个，最多的找到了11个。
我家儿子还不知道这也是考核内容，以为就是娱乐放松活动。。。。。。

补习问题:
SG 有问题问孩子有没有去补习学校？一般孩子们都是如实回答？
在笔试当天考试开始前会让孩子们填个表，其中就有问参加了哪些补习班，要求填写补习的具体时间补习社的名字，哪些课外活动. 
个人认为最好如实填写，即使因为这个拿不到奖学金，我们也绝不能教孩子撒谎，
最后一关面试时也会被问到这个事情，孩子一定要有底气说实话.
而且并不是大家说的补习了的孩子sg不愿意要，补习和补习还分情况了:
有的补习7年有的5年有的半年，有的在外面补有的在家补，你说学校怎么去评判这个补习呢。
所以孩子全面发展是硬道理，group activity后你会发现，
那些参加的孩子里有好些什么钢琴小提琴8级的，孩子们在一起玩会互相交流。
还有就是sg里面很多没有拿奖学金的孩子比有些拿了的厉害。
```
