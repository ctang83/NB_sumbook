---
title: R/Stata_Stated Preference
weight: 40
---

- How to refer to a variable name with spaces?  
答: backticks `` (反引号), 如模型:  
RES ~ `Indirect payment`+`Direct payment`+strata  
 ~ 符号在R里面是建立模型

- Hideo Aizaki online book [Non-Market Valuation with R](http://lab.agr.hokudai.ac.jp/nmvr/index.html)

**TO-DO-LIST**

1. randomization in displaying of attributes for each choice task
	- 文献里叫attributes ordering effects, Randomize the attribute order, 有几种情况:
	- (1)一个respondent加入,其n个choice set的attribute order随机化,实现难度大
	- (2)500个respondents,手动分2-5种attribute order版本的questionnaires, 再shuffle respondents随机进各版本
	- we can't do a complete randomization of attribute order, because some attributes are ususally grouped. 
1. formr [randomise items/blocks](https://github.com/rubenarslan/formr.org/wiki/How-to-randomize-items-and-blocks)
1. randomization of respondents across blocks, e.g. A-B-testing or experiments, or[adapt question texts to sex/age](https://github.com/rubenarslan/formr.org/wiki/How-to-adapt-question-texts-to-e.g.-the-participant's-sex-and-relationship-status)


**Literature**

1. [Attribute Selection for a Discrete Choice Experiment Incorporating a Best-Worst Scaling Survey](https://www.sciencedirect.com/science/article/pii/S1098301520345125#appsec1)


## DCEs Automation/Integration 


| pack/func | Features | MXL design | rand exp | create survey | run survey |
|:--------|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|
| formr   | 网站/包 |                |          | rand items |
| formr4conjoint |         |               |          |
| conjointTools  |         |               |          |
| idefix  |          | Bayesian adaptive |          | 
| DCEtool | local machine | 包装idefix |  |  
| ExpertChoice |  |  |

Table: Design, survey and analysis

通过搜索python conjoint, 终于找到合适工具

1. [conjointSDT.exe](https://github.com/astrezhnev/conjointsdt) Conjoint Survey Design Tool一个封装好的软件应用, 能实现attribute order randomization, 但是不知道里面DCE怎么设计的?没有D-optimal 
	- conjointSDT.py是文本文件, 试试找里面att order random的代码
	- 功能代码还是要到四件套里面找R coding
1. 上面conjointSDT推荐的估计模型R包[cjoint](https://cran.r-project.org/web/packages/cjoint/index.html)
1. [Pythonbiogeme](https://transp-or.epfl.ch/pythonbiogeme/), an open source freeware designed for discrete choice estimation.

1. Kaufman has a [Plos ONE](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0232424) and [GitHub](https://github.com/aaronrkaufman/shinySurveys) with focus on DCE ShinySurveys
    - 文章里实现4个功能,如DCE问题随机化, 但在RStudio Cloud和RStudio都页面闪退, 因此以后网调稳定还是很重要
1. Jonathan Trattner has a formal R package [shinysurveys](https://shinysurveys.jdtrat.com/) aims create and deploy surveys in 'Shiny'. *第1和第2重名但无overlap*
1. John Paul Helveston used formr.org to create a specific [formr4conjoint](https://github.com/ctang83/formr4conjoint)  
    - formr利用OpenCPU和Google form, 可能更稳定
  	- 如果formr无法用, 就直接用Google Forms, which可以实现DCE的[问题随机化](https://support.google.com/a/users/answer/9308764?hl=en)

### Design SP

Basic information for this part:

1. Orthogonal design 
1. fractional factorial design
1. efficient design (best choice)
		1. D-optimal design (optimal = maximally statistically efficient), 效率系数并非越大越好, 越大会降低[choice consistency](https://doi.org/10.1086/586913)

`install.packages("idefix")`, this R package combines designing DCE and dashboard implementing survey to collect data, all tasks can be conducted by using R and Github

```r
install.packages("remotes")
remotes::install_github("jhelvy/conjointTools")
```
[conjointTools](https://github.com/jhelvy/conjointTools), designing surveys and conducting power analyses for DCE,可以D-optimal设计,安装如下
*conjointTools*的作者John有一门特别好的课[Marketing Analytics for Design Decisions](https://madd.seas.gwu.edu/2021-Fall/c7-utility.html)

- help dcreate  //DCE设计Efficient designs

### Survey

`install.packages("DCEtool")`   
A complete package for creating, surveying, and analysing DCEs on **local machine**, including:
1. *idefix* to create the exp design, and present interactive survey by Traets et al. (2020)
1. *survival* and *mlogit* to analyse results
1. new code for visual interaction dashboard



## Estimating models

*case study*
This [one](https://rpubs.com/tmk1221/CBC) using R package to estimate Willingness to Pay, Preference Share Simulator, Sensitivity Plot

`install.packages("apollo")`
Coding and data examples using [Apollo](http://www.apollochoicemodelling.com/examples.html)

`install.packages("logitr")`  
This package can estimate [MNL and MXL](https://cran.r-project.org/web/packages/logitr/vignettes/mxl_models.html), derive WTPs from Preference space and WTP space.  
For details, see [random utility model in two spaces](https://cran.r-project.org/web/packages/logitr/vignettes/utility_models.html) 
 
`install.packages("mixl")`  
This R package for estimating complex choice models (MXL) on large datasets

% Latent Class(潜层模型) 

`install.packages("BayesLCA")`  
An R package for bayesian latent class analysis : Three methods for fitting the model are provided, incorporating an expectation-maximization (EM) algorithm, Gibbs sampling and a variational Bayes approximation.

- Stata mixed logit后计算支付意愿可用 mixlogitwtp 和 bayesmixedlogitwtp


## Binary DCE(used as Case 2 BWS) 

- 3.4.3 Organ donation部分 - Binary DCE(Lma method) BDCE可以立即用作 Case 2 BWS  
这段代码是Stata中运行, 调用R包, 生成DCE原始设计med数据库(med是R中由正交表自动生成的)
```r
rsource, terminator(END_R)
library(survival)  
library(stats)  
library(support.CEs)  
library(foreign)    # for Stata dataset output
  
  % 设计DCE的attrs-levels
med <- Lma.design( 
  attribute.names = list( consent = c("implicit", "active"),  final = c("no", "yes"), 
						  reg = c("various","unique"),     quit = c("never", "ask"),
						  death = c("brain","other"),      priority = c("no","yes"),
						  acknow = c("no","yes"),   payment = c("0","1w","2w","3w") ),
  nalternatives = 1,     # create design for a BDCE
  nblocks = 4,
  row.renames = FALSE,
  seed = 987)  
% med是由上面des产生的子数据库36x11，将18个choice set分解成36个alternatives(observations),
  % 共11个变量(2*5+1,5个离散变量，1个连续变量)描述一个alternative
  
attributes(med)   
head(med)  
questionnaire(choice.experiment.design = med ) 
data<-as.data.frame(med$alternatives$alt.1)   # med是list类型的对象, alt.1是attributes组合的对象
write.dta(data,"d:/test.dta")   # 输出BDCE attributes组合数据库
q() 

END_R

use d:\test.dta , clear
```
 
- 3.4.3 护理学生部分 Binary DCE(Lma method) BDCE可以立即用作 case 2 BWS 
```r
	rsource, terminator(END_R)

	library(survival)  
	library(stats)  
	library(support.CEs)  
	library(foreign)    # for Stata dataset output

	################## questionnaire #############################

	med <- Lma.design(
	  attribute.names = list(Place = c("Urban", "Rural"),      Facility = c("Hospital", "CHC"), 
							 Mgment = c("Fsuppot", "Psuppot"), Material = c("Fully", "Poorly"), 
							 Style  = c("Required", "Not"),    Career = c("Sufficient", "Limited"), 
							 Income = c("2000", "4000", "6000", "8000")), 
	  nalternatives = 1,     # create design for a BDCE
	  nblocks = 2,
	  row.renames = FALSE,
	  seed = 987)

	questionnaire(choice.experiment.design = med ) 
	as.data.frame(med$alternatives$alt.1)   # med是list类型的对象, alt.1是attributes组合的对象

	% PS: 2 blocks, 16 questions in total, thus each one answer 9 questions (including one consistent test)

	################## design matrix -- dm.med #############################

	- Because from the above questionnaire, we can create two types of BDCE: 
		- 1) No opt-out but include common alternative;
		- 2) Opt-out but no common alternative. 
	#  Here in DCE_med_spe, we choose 2). 

	dm.med <- make.design.matrix(
	  choice.experiment.design = med, 
	  optout = TRUE,    # include opt-out option
	  categorical.attributes = c("Place", "Facility", "Mgment", "Material", "Style", "Career" ), 
	  continuous.attributes = c("Income"), 
	  unlabeled = TRUE, # unlabeled design
	  common = NULL,    # do not include common alternative
	  binary = TRUE)    # BDCEs

	################## Datasets combine: final = res + dm.med #############################

	data(res)  # Input respondent dataset (clean the consistent test question before input)
	/*  res是这个格式的数据库
	  ID BLOCK q1 q2 q3 q4 
	1  1     1  1  1  0  0    
	2  2     2  1  0  0  1    
	3  3     3  1  0  0  1    
	4  4     4  0  1  0  0    
	5  5     1  0  1  0  0    
	6  6     2  1  1  0  1    
	*/
	final <- make.dataset(
	  respondent.dataset = res,     # respondent dataset
	  design.matrix = dm.med,       # design matrix
	  choice.indicators = c("q1", "q2", "q3", "q4", "q5", "q6", "q7", "q8"),
	  detail = FALSE)

	write.dta(final,"d:/test.dta")   # 输出BDCE attributes组合数据库

	################## Analysis ############################# 上面部分全部调试完毕，分析部分以后做

	END_R
```

- 3.4.3 医学生部分 BDCE , Lma method, BDCE可以立即用作 case 2 BWS
```r
rsource, terminator(END_R)
library(survival)  
library(stats)  
library(support.CEs)  
library(foreign)    # for Stata dataset output

################## questionnaire #############################

med <- Lma.design(attribute.names = list(
                  Place = c("Urban", "Rural"),      Facility = c("Hospital", "CHC"), 
                  Mgment = c("Fsuppot", "Psuppot"), Material = c("Fully", "Poorly"), 
                  Career = c("Sufficient", "Limited"),  Job  = c("Matched", "Nonmatched"),
                  Patient = c("Good", "Fair"),        Income = c("2000", "4000", "6000", "8000")), 
                   nalternatives = 1,     # create design for a BDCE
                   nblocks = 2,
                   row.renames = FALSE,
                   seed = 987)

questionnaire(choice.experiment.design = med ) 
as.data.frame(med$alternatives$alt.1)   # med是list类型的对象, alt.1是attributes组合的对象

# PS: 2 blocks, 16 questions in total, thus each one answer 9 questions (including one consistent test)

################## Matched design matrix -- dm.med #############################

# Because from the above questionnaire, we can create two types of BDCE: 1) No opt-out but include common alternative;
#   2) Opt-out but no common alternative. Here in DCE_med_spe, we choose 2). 

dm.med <- make.design.matrix(
  choice.experiment.design = med, 
  optout = TRUE,    # include opt-out option
  categorical.attributes = c("Place", "Facility", "Mgment", "Material", "Career", "Job", "Patient"), 
  continuous.attributes = c("Income"), 
  unlabeled = TRUE, # unlabeled design
  common = NULL,    # do not include common alternative
  binary = TRUE)    # BDCEs

################## Datasets combine: ds.med = medspe + dm.med #############################

data(medspe)  # Input respondent dataset (clear the consistent test question before input)

ds.med <- make.dataset(
  respondent.dataset = medspe,  # respondent dataset
  design.matrix = dm.med,       # design matrix
  choice.indicators = c("q1", "q2", "q3", "q4", "q5", "q6", "q7", "q8"),
  detail = FALSE)

head(ds.rural1)   # list head of the final dataset

################## Analysis 上面部分全部调试完毕，分析部分以后做 ############################# 

fm.rural <- RES ~ Agr + Env + Rec + Area + Tax
out.rural1 <- glm(fm.rural,           # is glm() the clogit in Stata ? 
                  family = binomial(link = "logit"),
                  data = ds.rural1)
summary(out.rural1)
gofm(out.rural1)
out.rural2 <- glm(fm.rural,
                  family = binomial(link = "logit"),
                  data = ds.rural2)
summary(out.rural2)
gofm(out.rural2)

mwtp.rural1 <- mwtp(output = out.rural1,
                    monetary.variables = c("Tax"),
                    nonmonetary.variables = c("Agr", "Env", "Rec", "Area"),
                    nreplications = 1000,
                    confidence.level = 0.95,
                    method = "kr",
                    seed = 987)
mwtp.rural1
mwtp.rural2 <- mwtp(output = out.rural2,
                    monetary.variables = c("Tax"),
                    nonmonetary.variables = c("Agr", "Env", "Rec", "Area"),
                    nreplications = 1000,
                    confidence.level = 0.95,
                    method = "kr",
                    seed = 987)
mwtp.rural2

str(mwtp.rural1$mwtps)
MWTP_Rec1 <- mwtp.rural1$mwtps[, "Rec"]
MWTP_Rec2 <- mwtp.rural2$mwtps[, "Rec"]
mded.rec <- mded(distr1 = MWTP_Rec1,
                 distr2 = MWTP_Rec2,
                 detail = TRUE) 
mded.rec
par(mfrow = c(1, 2))  # not shown in SPMUR
hist(mded.rec$diff, main = "(a) Rec", xlab = "Difference")

MWTP_Area1 <- mwtp.rural1$mwtps[, "Area"]
MWTP_Area2 <- mwtp.rural2$mwtps[, "Area"]
mded.area <- mded(distr1 = MWTP_Area1,
                  distr2 = MWTP_Area2,
                  detail = TRUE) 
mded.area
hist(mded.area$diff, main = "(b) Area", xlab = "Difference")

v0 <- 0
Plan1 <- c(1, 0, 0, 0, 20, 0)
v1 <- sum(out.rural1$coef * Plan1)
(-1 / -out.rural1$coef["Tax"]) * (v0 - v1) 

Plan2 <- c(1, 1, 0, 0, 60, 0)
v2 <- sum(out.rural1$coef * Plan2)
(-1  / -out.rural1$coef["Tax"]) * (v0 - v2)

set.seed(123)
COEF <- mvrnorm(out.rural1$coef,  # coefficients
                vcov(out.rural1), # variance-covariance 
                n = 1000)         # number of replications

V0 <- rep(0, 1000)
V1 <- COEF %*% Plan1 # %*% is matrix multiplication
C1 <- (-1 / -COEF[, "Tax"]) * (V0 - V1)
V2 <- COEF %*% Plan2
C2 <- (-1  / -COEF[, "Tax"]) * (V0 - V2)

hist(C1, main = "Case 1", xlab = "Compensating variation")
hist(C2, main = "Case 2", xlab = "Compensating variation")

quantile(C1, c(0.025, 0.975))
quantile(C2, c(0.025, 0.975))

(-1 / -out.rural1$coef["Tax"]) *
  (log(exp(v0) + exp(v1)) - log(exp(v0) + exp(v2)))

par(mfrow = c(1, 1))       # not shown in SPMUR
C3 <- (-1 / -COEF[ ,"Tax"]) * 
  (log(exp(V0) + exp(V1)) - log(exp(V0) + exp(V2)))
hist(C3, main = "", xlab = "Compensating variation")
quantile(C3, c(0.025, 0.975))
```


## case 1 BWS

- Two methods for constructing choice sets for case 1 BWS:  
	- OMED - two level orthogonal main-effect design  
	- BIBD - blanced incomplete block design (每题的number of items固定，所以建议只用这种)BIBD 可用的difference set如下

```r
find.BIB(trt = 16, k = 6, b = 16 )
find.BIB(trt = 15, k = 7, b = 15 )
find.BIB(trt = 13, k = 4, b = 13 )  # 11 or 13 items is the best!
find.BIB(trt = 11, k = 5, b = 11 )
find.BIB(trt = 10, k = 4, b = 15 )
find.BIB(trt = 9,  k = 3, b = 12 )
find.BIB(trt = 8,  k = 4, b = 14 )
find.BIB(trt = 7,  k = 3, b = 7  )
find.BIB(trt = 6,  k = 3, b = 10 )  # 总共6个items，10 个问题，每个问题3个items

########### 用于7，9,11个items的case 1 BWS  
items <- c("1", "2", "3", "4", "5", "6", "7")
items <- c("1", "2", "3", "4", "5", "6", "7", "8", "9")  
items <- c("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11")
```

- Organ donation - Results analysis by R (all output in D:\) , 所有结果输出都Stata化了, 只需要解决第2部分Create dataset: "final" = res + bibd

```r
rsource, terminator(END_R)
#######################  下面是成熟的针对13个items的程序，只需加载第2部分的新res数据库（res是来自调查的数据）
library(DoE.base)
library(crossdes)  #  crossdes package中的 find.BIB函数产生BIBD设计
library(support.BWS)
library(survival)
library(foreign)
###########################################################
%%% 1. Constructing choice sets & Case 1 BWS questions (BIBD) - organ donation 器官捐献量表
###########################################################

set.seed(123)
bibd <- find.BIB(trt = 13, k = 4, b = 13)   # BIBD设计
isGYD(bibd)   # 此函数检查 trt，k，b 搭配是否平衡
%View(bibd)
items <- c("Media", "Religion", "Tradition", "Legislation", "Family", 
           "Experience", "Self-decision", "Consent", "Withdrawal", "Death", 
           "Allocation", "Indirect payment", "Direct payment")

%接下来两步走，准备问卷和创建final数据库是独立的，
%%% 1). Preparing BWS questions   # design.type = 2 means BIBD

% bws.questionnaire(choice.sets = bibd,  design.type = 2,   item.names = items )  

%%% 2). Create dataset "final" = res + bibd

res<-read.dta("D:/desk/Googledrive/fj_phs/data/res.dta")

final <- bws.dataset(respondent.dataset = res,
                   response.type = 1,  # row number format
                   choice.sets = bibd,
                   design.type = 2,    # BIBD
                   item.names = items) 
write.dta(final,"d:/BWS_final.dta")

%View(final)   # list dat 数据库，此数据库是最终用于分析的库

###########################################################
%%% 2. Analyzing responses using "The Counting Approach" (output: 3 graphs)
% two tables: 1.Disaggregated best-worst scores & 2.Aggregated best-worst scores
% 表1,2的关系是，表1所有系数乘以n (n是被调查人数)
% PS: 分析部分需要Stata化，R的分析完全没有过程！
###########################################################

scores <- bws.count(data = final)
scores
scores$aggregate

data<-as.data.frame(scores$aggregate)   # med是list类型的对象, alt.1是attributes组合的对象
write.csv(data,"d:/tab_BWS_aggregate_score.csv")  # 这个表待会读入Stata，然后转成Latex表

%%%%% Figure 1 Distribution of simple BW scores by items
          BW <- data.frame(scores$disaggregate$BW)
          BW <- lapply(BW, factor,  levels = c(-4:4))  # scores are transformed into a factor
score.tables <- lapply(BW, table)                      # table for each item
      ylimit <- max(unlist(score.tables))              # max count among items

pdf("d:/fig_BWS_bars_items.pdf")  # open .pdf for output
par(mfrow = c(4,4))    # create 4 x 4 figure regions, otherwise alarm of "out of figure margin"
for(i in 1:13){        # draw graph for item 1-13
  barplot(height = score.tables[[i]],
          main = items[i], # title
          xlab = c("BW score"),  # label for x-axis
          ylab = c("Count"),     # label for y-axis
          ylim = c(0, ylimit))   # limits for y-axis
}
dev.off()  # output graph with pdf type

%%%%%% Figure 2 Relationship between the mean BW score and its standard deviation
              Mean.BW <- colMeans(scores$disaggregate$BW)
Standard.Deviation.BW <- apply(scores$disaggregate$BW,  2, sd)

pdf("d:/fig_BWS_mean_SD.pdf")
plot(x = Mean.BW,  y = Standard.Deviation.BW,  xlim = c(-3, 3),  ylim = c(1, 2))
text(x = Mean.BW,  y = Standard.Deviation.BW,  pos = 4,    labels = items)
dev.off() 

%%%%%% Figure 3 Mean BW score per item in each cluster
set.seed(123)
kmeans <- kmeans(x = scores$disaggregate$BW, centers = 2)
kmeans$centers

pdf("d:/fig_BWS_mean_ranking.pdf")
barplot(height = kmeans$centers, 
        names.arg = items , 
        beside = TRUE,     # draw juxtaposed bar
        legend = c("cluster.1", "cluster.2"), 
        horiz = TRUE,
        las = 2,
        xlab = "BW score", 
        xlim = c(-4, 4))
dev.off()

END_R
```


- 读入tab_BWS_aggregate_score.csv 然后用mata转成Latex表
```r
import delim using "d:\tab_BWS_aggregate_score.csv"  , clear
gen att_id=_n 
rename v1 att_name
order att_id , b(att_name)
* tex output
texsave _all using "d:\tab_BWS_aggregate_score.tex" , frag replace  ///
		title(Aggregated best-worst scores)  ///
		footnote(Note: The number of respondents is 190)  
```

- Analyzing responses using the modeling approach
```r
use  "d:\final.dta" , clear

fr <- RES ~ A + B + C + E + F + G + H + I + J + K + L + M + strata(STR)   
    % the model exclude D to normalize its coefficient to 0
clogit(formula = fr, data = dat)
```


