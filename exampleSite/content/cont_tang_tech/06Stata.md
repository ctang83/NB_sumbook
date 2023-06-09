---
title: Stata
weight: 60
---
 

基于putdocx的4个命令： reg2docx, sum2docx, t2docx，corr2docx

help cfvars 文件1 文件2  //自动比较两个数据库的变量

labelbook   //列所有标签

findname //List variables matching name patterns or other properties

ssc install mkduration //create duration variables in binary cross-sectional time-series data

notes: xxxx //把xxxx加到当前dta

gsort +xx 或者 -xx  //排序时sort只能一个方向，gsort可以两个

by group,sort: sum age  //按组group统计age

gen no=_n   //编号

browse if oops>1    //窗口查看,替代list命令

cap noisily reg y x  //声明错误,但程序继续运行完

viewsource XXX.ado  //看源程序

f(%3.2f)  //小数点后两位数格式

duplicates drop XXX, force  //删除重复values检测

clonevar newvar = varname  //复制创建新变量, var label和value label也一起复制过去

## Debug问题集

set trace on  //debug
set trace off

- 粘贴导入excel数据库时，遇到不将first row treat as var name原因：1. 第一行变量名有空格，2. 或用了long等Stata保留词

- 用tab icd10遇到unique值太多的时候，又只想看频率最多的前10个
```r
groups icd10, order(h) select(10)
groups  xx  ,  saving(tab1, replace) //生成唯一值表，用于合并

help icdpic   //处理ICD 9-10 命令族, 最下面有一系列Also see
```

- missing data 缺失值/数据
```r
mdesc _all //关注缺失组和整体组的核心变量是否有显著性差异, 以此简单判断-缺失是否具有选择性
fmiss  varlist, detail

把缺失值转换成数值，以便于数据的各种转换
mvencode _all, mv(999)    //Translate missing values in the data to 999
mvdecode _all, mv(999)    //Change 999 values back to missing values

Stata 多个变量找缺失值
egen nmis=rmiss2(landval improval totval salepric saltoapr)
tab nmis   //4个obs没有缺失值, 9个obs有1个缺失值  http://www.ats.ucla.edu/stat/stata/faq/nummiss_stata.htm
```

- 合并/match/merge/系列命名dta
```r
	qui fs xx.dta      //先list文件名，存进r(files)
	foreach f in `r(files)' {   
	qui append using `f'
	}

- 3个命令模糊match or fuzzy merge:
  reclink  2010
  strgroup  2012 , 使用了merge后,针对_merge==3用strgroup
  nearmrg   2012

tempfile  master  part1  // 使用
save "`master'" , replace
append using "`part1'" 

mmerge   //多文件匹配
```

- levelsof varname [if] [in] [, options] // 显示变量values,并排序levels
```r
//e.g.按每个变量值（880种病）导出
	cd  "d:\icd"
	levelsof icd10c , local(levels)
	qui foreach l of local levels {
		export excel using "`l'.xlsx"  if icd10c=="`l'" , first(var) replace 	
		drop  if icd10c=="`l'" 
	}
	
//e.g.
  sysuse auto
  replace make=substr(make, 1, 4)
  levelsof make , clean separate(;) local(make) matcell(freq)
  display "`make'"
  matrix l freq
```

- 数出每个单位有几个医生spe<4，赋值给各单位医生数变量pn。其a和b是中间产生的变量。
```r
gen a=1 if spe<4
bysort org: replace pn=sum(a) if spe<4
bysort org: egen b=max(pn)     //egen是bysort org全部加总
bysort org:  gen b=max(pn)      //gen是逐个加总
replace pn=b
drop a b

//另一方法:用egen
egen     abc=group(date hour shi)
egen racesex=group(race sex) //racesex 交叉组, and containing missing if race or sex are missing

egen zong=noccur(c),s(",")  //counting number of times a string appears in the variable某字符在变量中出现的次数
egen store_occupy_mean_price=mean(price)			  , by(store_id  occupy_id)
egen f_dropout_kids_only=count(student_id) if f_edu<12, by(school)
egen f_dropout_kids_only=tag(student_id)   if f_edu<12, by(school)  //create dummy var

egen g=group(x)
sum g
list r(max)
```

## Modelling

help simex  // 做policy simulation，还有help simexplot

help meta  //元分析Stata-summary estimates Forest plot of study-specific estimates 合并效应值的森林图

coefplot -- plotting regression coefficients and other estimates

- 转平衡面板from unbalanced to balanced
```r
	encode hosid , gen(hid)  //字符hosid转数值hid
	xtset hid ym   //定义面板 hosid ym
	xtpatternvar, gen(pattern) //转平衡面板
	codebook patt
	fre patt , as
```

- 分析幂律, 私人组件 powerlaw（已装） http://coin.wne.uw.edu.pl/mbrzezinski/software
```r
doedit example.do //使用powerlaw，没有help文件，但有例子example.do
见NoteExpress： Brzezinski, M. 2014. Do wealth distributions follow power laws? Evidence from `rich lists'. Physica A 406: 155-162.

- Fitting Data into Power Distribution: Estimating alpha and x0. 
It's not a matter of estimating the 2 parameters together in one step
Fit your model using -paretofit- for multiple thresholds over a plausible range, and from this, calculate the K-S statistics.

* 分析分布
help pwlaw  //基于一个变量，计算power统计量Stata journal  D:\aa\LITERATURE\POOL-2014\10.1177@1536867X20953571.pdf
help    qplot //有power law, 见NoteExpress：Speaking Stata: The protean quantile plot. Stata Journal, 2005. 5(3): p. 442-460.
help distplot	//Do wealth distributions follow power laws?  D:\aa\LITERATURE\分级诊疗-自科\幂律和zipf\brzezinski2014.pdf

- R
poweRlaw: Analysis of Heavy Tailed Distributions
```

- zero inflated regression (most Y zero)
help zinb //Zero-inflated negative binomial regression (Y is a nonnegative count var)  
help zip  //Zero-inflated Poisson regression

Durbin–Watson test 自相关autocorrelation检测
```r
estat dwatson // Performs Durbin-Watson test of residual autocorrelation following regress. The data must be tsset
因为自相关系数ρ的值介于-1和1之间，所以 0≤DW≤４
DW＝ O   ＜＝＞  ρ＝１　　即存在正自相关性
DW＝４   ＜＝＞  ρ＝-１　即存在负自相关性
DW＝２   ＜＝＞  ρ＝０　　即不存在（一阶）自相关性
　　因此，当DW值显著的接近于O或４时，则存在自相关性，而接近于２时，则不存在（一阶）自相关性。
```

- logit回归，输出表格以Odds ratio (OR) 即 exponentiated coefficient形式加z检验系数（eform）或输出 Confidence Interval (CI), 
```r
esttab urban  urbanfe  $usertf ,  $tab replace  nonum  mtitles("Logit model" "Logit model with FE") ///
            cells(b(star fmt(%9.3f)) ci(par))  keep(2.edu 3.edu age gender)    eform z   

经过比较后,感觉很好的输出格式: (但, eform表示mlogit输出RRR-自然对数, tex输出Latex格式)

esttab m2 , drop(o.* 1b.* 2o.* 3o.* 4o.*)  [eform] unstack replace  [tex]  ///
            cells(b(star fmt(%9.3f)) se(par))   starlevels(* 0.10 ** 0.05 *** 0.01)  ///
            stats(N chi2 bic, star(chi2) fmt(%9.0g %9.3f))   varlabels(_cons Constant) 
            
如果输出 margins
margins, dydx(_all) post  // post option that tells margins to leave the results behind as if it were an estimation command
esttab using x.tex, label  

esttab  , r2 ar2 pr2  //可以调出显示model的R2，Adj-R2，Pseudo-R2

```

            
- FE固定效应
```r
大于2-level fixed effect in one model：
egen industry_firm=group(industry firm)  // or more FE var
方法一
regress y x1 x2 x3 x4  i.industry_firm
方法二
areg y x1 x2 x3 x4, absorb(industry_firm) // 交叉项产生2-level FE，8x9type=72 个dummies
方法三(可2个以上dummy变量)
aareg y x1 x2 x3 x4, absorb(industry firm) // 分开不等于交叉项， 8+9type=17 个dummies 
areg -- Linear regression with a large dummy-variable set


tabulate province,gen(dumy)  // 就可以产生dumy1－dumy31变量，
reg y x1 x2 dumy2-dumy31   // 或者不产生，在回归的时候用xi命令
xi: reg y x1 x2 i.province
```

- Stata已经发展了一套"截断时序回归"命令
```r
`help itsa  
help itsamatch
help itsaperm
```

- help regcheck  //同时检验所有回归假设

- 标记参与了回归的样本
```r
reg price rep78 weight length foreign
gen esample=1 if e(sample) //标记参与了回归的样本
su price rep78 weight length foreign if esample==1
```

- event study事件研究
help eventdd  //画图特别好  
其他命令  estudy  eventstudy  eventstudy2   
help eventstudy2

- 处理多期的一般DID, for causal analysis with varying treatment time and duration
help flexpaneldid
help flexpaneldid_preprocessing
help bacondecomp  //Bacon decomposition of DID estimation with variation in treatment timing

- help forecast  // a suite of commands for obtaining forecasts/projection by solving models

- 小聪的DID命令
```r
webuse nlswork  //使用系统自带数据库
xtset idcode year, delta(1)  //设置面板
xtdescribe   //描述一下这个面板数据情况

gen age2= age^2
gen ttl_exp2=ttl_exp^2
gen tenure2=tenure^2

global xlist "grade age age2 ttl_exp ttl_exp2 tenure tenure2 not_smsa south race"
sum ln_w $xlist  //统计描述相关变量

**DID方法-----------------------------------
gen time = (year >= 77) & !missing(year)  //政策执行时间为1977年
gen treated = (idcode >2000)&!missing(idcode) //政策执行地方为idcode大于2000的地方
gen did = time*treated  //这就是需要估计的DID，也就所交叉项

reg ln_w did time treated $xlist //这就是一个OLS回归，也可以用diff命令
xtreg ln_w did time treated $xlist i.year, fe //也可以这去做，会省略掉一个虚拟变量

**PSM-DID方法-------------------------------

** PSM的部分
set seed 0001	//定义种子
gen tmp = runiform() //生成随机数
sort tmp //把数据库随机整理
psmatch2 treated $xlist, out(ln_w) logit ate neighbor(1) common caliper(.05) ties //通过近邻匹配，这里可以要outcome，也可以不要它
pstest $xlist, both graph  //检验协变量在处理组与控制组之间是否平衡
gen common=_support
drop if common == 0  //去掉不满足共同区域假定的观测值
psgraph

** DID的部分，根据上面匹配好的数据
reg ln_w did time treated $xlist 
xtreg ln_w did time treated $xlist i.year, fe

**PSM-DID部分结束--------------------------------------

*交互项 
help xi 
**DID方法需要满足的五个条件检验------------------------

**1.共同趋势假设检验

tab year, gen(yrdum) //产生year dummy，即每一年一个dummy变量
     forval v=1/7{
gen treated`v'=yrdum`v'*treated
}                     //这个相当于产生了政策实行前的那些年份与处理虚拟变量的交互项
xtreg ln_w did treated*  i.year ,fe  //这个没有加控制变量
xtreg ln_w did treated* $xlist i.year ,fe //如果did依然显著，且treated*这些政策施行前年份交互项并不显著，那就好
xtreg ln_w did treated* $xlist i.year if union!=1 ,fe //我们认为工会会影响这个处理组和控制组的共同趋势，因此我们看看union=0的情形

**2.政策干预时间的随机性
gen time1 = (year >= 75) & !missing(year)  //政策执行时间提前到1975年
capture drop treated1
gen treated1= (idcode >2000)&!missing(idcode) //政策执行地方为idcode大于2000的地方
gen did1 = time1*treated1  //这就是需要估计的DID，也就所交叉项

gen time2 = (year >= 76) & !missing(year)  //政策执行时间提前到1976年
capture drop treated2
gen treated2= (idcode >2000)&!missing(idcode) //政策执行地方为idcode大于2000的地方
gen did2 = time2*treated2  //这就是需要估计的DID，也就所交叉项

xtreg ln_w did1 $xlist i.year,fe 
xtreg ln_w did2 $xlist i.year,fe //看看这两式子里did1和did2显著不,显著为好

**3.控制组将不受到政策的影响
gen time3 = (year >= 77) & !missing(year) 
capture drop treated3 
gen treated3= (idcode<1600 & idcode>1000)&!missing(idcode) //我们考虑一个并没有受政策影响地方假设其受到政策影响
gen did3 = time3*treated3  
xtreg ln_w did3 $xlist i.year,fe //最好的情况是did3不显著，证明控制组不受政策影响


**4.政策实施的唯一性，至少证明这个政策才是主要影响因素
gen time4 = (year >= 77) & !missing(year)  
capture drop treated4
gen treated4= (idcode<3000 & idcode>2300)&!missing(idcode) //我们寻找某些受到其他政策影响的地方
gen did4 = time4*treated4  
xtreg ln_w did4 $xlist i.year,fe //did4可能依然显著，但是系数变小，证明还受到其他政策影响

**5.控制组和政策影响组的分组是随机的

xi:xtivreg2 ln_w (did=hours tenure) $xlist i.year,fe first //用工具变量来替代政策变量，解决因为分组非随机导致的内生性问题

————————————————————————————————
**附加的，一般而言，我们需要看看这个政策的动态影响-------------
     forval v=8/15{
gen treated`v'=yrdum`v'*treated  
}               //注意，这里yrdum8就相当于year=78
	
reg  ln_w treated*  

xtreg  ln_w treated*, fe

xtreg ln_w treated* i.year,fe 

xtreg ln_w treated* $xlist i.year,fe  //一般而言上面这些式子里的treated*应该至少部分显著


倍分法 (DID)多个现成命令：
help diff   //sj16-1
help ddid   //Pre-post-treatment estimation of the Average Treatment Effect (ATE) with binary time-varying treatment
didq        //Treatment-effect estimation under alternative assumptions, SJ 15(3):796--808
help absdid //Semiparametric difference-in-differences estimator of Abadie (2005), SJ16-2, 附数据和范例
——————————————————————————————————————
* RDD 多个断点--包括分析、画图、稳健 Analysis of regression-discontinuity designs with multiple cutoffs or multiple scores
https://journals.sagepub.com/doi/pdf/10.1177/1536867X20976320

**RDD断点回归在这里的应用-------------------

tab treated,missing
keep if treated==1
cmogram ln_w year,cut(77) scatter lineat(77) qfitci //绘制断点回归图形

**最优带宽
set more off
rdbwselect ln_w year if 60<=year&year<=85,c(77) kernel(uni) //自己下载安装
rdob ln_w year, c(77)                      //这个命令是imbens新开发的，这里有rdob的程序

reg ln_w time $xlist if 60<year&year<80  //直接对time这个虚拟变量做了ols
```

### inequality不平等
/*第一步：计算Gini 或 CI （常常画图）
  第二步：分解
	1）动态or面板：decomposition of a change in inequality 
		   -- Stata命令：dsginideco(2017) - Decomposition of inequality change into pro-poor growth and mobility components
						 adecomp(2012) - Shapley Decomposition by Components of a Welfare Measure
	2）截面
		   -- Stata命令：INEQDECO--Inequality decompositions by subgroup
						 trnbin0 -- count data Yvar 集中指数非线性分解法
						 ineqrbd -- Regression-based inequality decomposition, following Fields (2003) 
						 ineqfac(2009) -- Inequality decomposition by factor components, following Shorrocks (1982a, b)
						 fgt_ci -- calculates and decomposes Foster–Greer–Thorbecke (and standard) concentration indices
						 rifireg(2016) - Recentered Influence Function (RIF) for (bivariate) rank dependent indices (I) regression */

1.gini decomp:   "ginidesc" , "ineqdeco" ,  "ineqdec0"
1.各种系数计算:  "inequal"→ "inequal7"→升级→"ainequal"		 
								 
## Plot/graph/visualisation

help stripplot  //画图比较多个分组的均值和分布

- 正态检验画图
  - sktest var    // 正态检验, p<0.00 则var不是正态
```r
	help diagnostic_plots
    swilk  x  // z>2.14代表x不服从正态； p<0.01代表不服从正态
	pnorm  x  // 分布偏离斜线，也不服从正态。
	gen u=rnormal(8, 3)  //检验一下正态
```

- 现在的可视化工具webGL(graphical language)：一还是基于我们单机的统计分析；二优点在于"交互性"和"面向网络"；
  - D3是基于JS：javascript, JSON=JavaScript Object Notation
  - Plotly绘图底层基于D3
```r
help libd3 //a Mata wrapper around the D3 JavaScript library
help libjson
help libhtml	
```

- Longitudinal data可视化
help xtline
help xtgraph

- boxplot箱线图，https://wiki.mbalib.com/wiki/%E7%AE%B1%E7%BA%BF%E5%9B%BE
gr box x1 x2  //看几个变量(样本)是否具有有对称性，分布的分散程度。

- 3个命令Plots of mean CI over time 
```r
twoway lfitci yvar xvar
help caterpillar 
help ciplot  //这种图也叫 Caterpillar 

ciplot yhb , by(ym) //医护比
```

- graph combine 1.gph 2.gph , ysize(2) xsize(4)  //合并时，调整graph图的纵横比

sixplot var //精简散点图, 一次出6连图（描述分布、正态、柱状）：

binscatter  //一个相见恨晚的stata命令

- 将这句three-way画图：tab ym hoslev , sum(wt) nost nofre 
```r
egen meanwt=mean(wt), by(ym hoslev)
	separate meanwt, by(hoslev) 
	twoway connected meanwt? ym2, c(L L) sort
	
	egen meancmi=mean(cmi), by(ym hoslev) //分类分组求均数，然后按年月画图连线
	separate meancmi, by(hoslev) 
	twoway connected meanwt? ym2, c(L L) sort title(DRG相对权重) xtitle(年月) ytitle(平均值) msymbol(S Oh) xline(201803) legend(label(1 "二级医院")label(2 "三级医院"))
```	


- 画图 + code修图
```r
sysuse auto, clear
twoway scatter mpg rep78, msize(small) ||, graphregion(margin(r+50)) yti("里程数") xti("1978年的维修次数")

gr_edit AddTextBox added_text editor `=82+8' `=101'
gr_edit added_text_new = 1
gr_edit added_text[1].text = {}
gr_edit added_text[1].text.Arrpush "Mean mpg by rep78"
gr_edit AddTextBox added_text editor `=82+2' `=112'
gr_edit added_text_rec = 2
gr_edit added_text[2].text = {}
gr_edit added_text[2].text.Arrpush "rep78  mpg"
```

- 常用画图
```r
twoway (scatter read write) (lfit read write) (kdensity pr1) //散点分布，回归拟合图

help mrgraph  //分类变量画图
help mrtab

同时显示多个窗口graph
grss twoway histogram price, frequency bin(20)
grss twoway histogram weight, frequency bin(20)

茂睿的类似阴阳hist图
sysuse nlsw88
twoway (hist wage if union==1, frequency color(gs11) lwidth(none)) ///
			 (hist wage if union==0, frequency color(none) lwidth(medium) lcolor(navy)  legend(order(2 "Non-union" 1 "Union")))

单变量--探索分布时，可用画图hist命令，非常直观
hist fee if own==1 & icd10=="O80.901" & fee<6000 , bin(500) kden  name(a)
hist fee if own==1 & icd10=="O80.001" & fee<6000 , bin(500) kden  name(b)
gr combine  a b c d e f , row(3) col(2)

help kdensity  //画kernel图更好，两个命令，第一个是官方命令
help kdens

第一次用stata做直方图，其实命令比想象中简单，开始搞复杂了，而且发现也可以跟随stata的界面操作学习命令！直方图命令：
graph bar (count) city1, over(city, gap(20))   ytitle("卫生人员数量") b1title("各设区市") blabel(bar)
```

### 地理地图map

- Extracting Chinese geographic data from Baidu Map API [中国地图地理](https://journals.sagepub.com/doi/pdf/10.1177/1536867X20976313)
```r
help cngcode //定位
help cnaddress  //经纬度转中文地址

help cnmapsearch //根据关键词搜索给定经纬度周边的兴趣点，返回兴趣点的名称、中文地址、距离、标签等。 
help chinagcode  //利用百度地图获取中国地址的经纬度

help cntraveltime 
help grmap  //12以前叫spmap
```

- 以spmap例子解释
```r
 use "Italy-RegionsData.dta", clear  //打开"数据-库"
 spmap relig1 using "Italy-RegionsCoordinates.dta", id(id) //"底图库" 和 "数据-库"以id变量唯一识别link
                                                           // 原理：将"数据-库"中 relig1 变量 plot在"底图库"上面
*转地理数据into Stata库
shp2dta  -- reads a shape (.shp) and dBase (.dbf) file //栅栏文件shp和dbf都需要
mif2dta  -- converts MapInfo Interchange Format files (aka MIF files) to Stata 

*地理经纬
geocode3 -- google升级到API3了, 其利用insheetjson和libjson(json library)两包解析网页,再返回给mata
geocode3命令不认汉字, 只认拼音或汉字的utf-8编码
nearstat //计算距离distance命令
```

### 改scheme
help schemes

set scheme plotplain  //我觉得最像R ggplot图风
set scheme Plottig, permanently  // 对Stata单色模板的改进
set scheme  s2mono, permanently  //s2monochrome 是我一直用的喜欢scheme

sysuse auto
histogram price, scheme(michigan) percent title("Automobile Prices")  // 非常漂亮图模

### Output graph

Stata figure出图终极解决方案--使用Ghostscript-gswin64c.exe：

win10环境下安装Ghostscript, [Step I](https://mile.com/assets/documents/Installing-Ghostscript-on-Windows-10.pdf). 
win10环境下根据[Step II](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/)添加
`C:\Program Files\gs926\bin`, 重启电脑就行了

方法：Stata出矢量pdf，然后Stata调用!gswin64c.exe 将pdf转tiff (也可设定dpi=r600，和压缩方式lzw)，我包装为程序figen

```r
  figen, fig(D:\fig1) // 括号中=路径和名称
  doedit "D:\stata11\ado\myado/figen.ado" //看ado,这个命令已定型，切勿再改
  
  cap prog drop figen
  prog figen 
  syntax,  [if] [in]  fig(str)
       graph export "`fig'.pdf"  
       !gswin64c.exe -dSAFER -dBATCH -dNOPAUSE -r600 -sDEVICE=tiff24nc -sCompression=lzw  -sOutputFile="`fig'.tiff"  "`fig'.pdf"
       erase "`fig'.pdf" 
  end
```

## EDA探索性分析

三适合探索新变量: 
des _all
sum _all
tab xx  
fre xx
ins xx

codebook xx  ////怎么探索字符变量？告诉missing值、有多少unique values

order varlist  //左下框内变量排位置用order和move命令

sort name year //在排序的时候可以用上两个变量，这样先name后year就不会乱了year

sort X, stable //而sort加选项stable可以使排序最简洁最快

- 比较灵活的fsum
fsum transaction  if n==600578 & date>date("2007-05-20","YMD") & date<date("2007-05-31","YMD") ,f(11.0)  //日期
fsum , stats(n) label //输出所有var和var label

- 描述性分析：  
```r
	cap !erase 2.csv // 省去手动删excel文件
	 // categorical var
	foreach v of varlist hl hisb_hc age edu gender marital $ill { 
		quiet estpost tab `v' if !mi($nomis)
		quiet esttab using 2.csv, cells(" b(label(freq)) pct(fmt(2))") unstack nonumber noobs append 
	    } 
	 // continuous var
	quiet estpost sum w if !mi($nomis)
	quiet esttab using 2.csv, cells(" mean(fmt(2)) sd(fmt(2)) ") unstack nonumber noobs append 
```

### Output输出

table输出Excel(csv)终极解决方案：
```r
`	foreach v of varlist $covs { 
		  estpost tab `v' cl , chi2  // cl=cluster，是分层指示变量
	quiet esttab  using "$path\BWS_organ\new_survey\tab_BWS_aggregate_score.csv", cells("b(label(freq)) colpct(fmt(2))") unstack nonumber noobs append 
			} 
	esttab  CL   using 2.csv, mtitles("Conditional Logit")  nogaps pr2 scalars(ll r2_p ) nodep nonum se unstack append
```

table输出Word(rtf)终极解决方案：
```r
`	esttab urban  using 2.rtf  ,  unstack replace  nonum    ///
		mtitles("Logit model")   ///
		starlevels(* 0.10 ** 0.05 *** 0.01)   addnotes("* p<0.10, ** p<0.05, *** p<0.01")   ///
        cells(b(star fmt(%9.3f)) se(par))      ///
		stats(N ll chi2 r2_p bic, star(chi2) fmt(%9.0g %9.3f))   ///
		varlabels(edu "Higher education"  age "Age"  gender "Male"  _cons Constant) 


esttab 导出latex table，需要\label{tab2}, 直接在esttab选项里 title(...\label{tab2})
```

- 读入tab_BWS_aggregate_score.csv 然后用mata转成Latex表
```r
`import delim using "d:\tab_BWS_aggregate_score.csv"  , clear
gen att_id=_n 
rename v1 att_name
order att_id , b(att_name)
* tex output :  texsave把Stata数据库中数据做成latex表格
texsave _all using "d:\tab_BWS_aggregate_score.tex" , frag replace  ///
		title(Aggregated best-worst scores)  ///
		footnote(Note: The number of respondents is xxx) 
```

- 输出表格table命令汇总：
1) summary矩阵表格化tabulating matrix as publication  
frmttable 其可以利用outreg的选项，如annotate(pcts) asymbol("\%")加百分号，还可以自由改表头

2) estimates矩阵表格化          
estout 或者 frmttable, outreg - outreg太多bugs，常需要run几次才能出一个表格  
esttab最推荐*****，因为esttab可以调用estout(太麻烦不推荐)的选项
```r
Summary statistics → plain text and LATEX files  with tabout (Watson 2007), outreg2, estpost(distributed with the estout)
estimation tables  → Word and LATEX tables       with outreg (which uses frmttable for formatting) 
                   → plain text and LATEX tables with outreg2 (Wada 2005), estout(Jann 2007), esttab(a wrapper for estout)
                   → formatted Excel file        with xml-tab (Lokshin and Sajaia 2008) ，选择性输出改动excel： putexcel(Stata 13)
 * 最强的(可将matrix完美输出成table)：
estimation/Summary → Word and LATEX tables       with frmttable (gallup 2012) , outtable (Baum and Azevedo 2008)
                                                 outreg(gallup 2013)是frmttable的升级包装版，outreg可用frmttable的选项

 * 教你各种表格（可自动run命令）
help tabletutorial
 * 输出后各类型文件转换、编译
help translate  //-- Print and translate logs
help texify   // 等价于 !pdflatex myoutput.tex -no-shell-escape
help ttab   // ttab is basically a wrapper for ttest and estout. 大规模ttest时用

• The "estout package" contains four commands:
esttab: User-friendly command to produce publication-style regression tables for screen display or in various export(CSV, RTF, HTML, or LaTeX)
estout: Generic program to compile regression tables (engine behind esttab)
estadd: Program to add extra results (such as beta coefficients) to e() so that they can be tabulated.
eststo: Improved version of estimates store.
```

- LaTeX and Stata integration: 很漂亮的表
```r
今天自编gr2tex(利用texdoc) ：一输出graph为pdf保存；二将引用latex code插入draft
global path  D:\desk\Googledrive\fj_pilot_organ
cd $path 
sysuse auto
scatter price wei
gr2tex , pdf($path\draft\output\aa)  draft($path\draft\bws_organ)


tex3pt -- creates LaTeX documents from esttab output using the LaTeX package threeparttable.
texdoc -- write into tex file. (texdoc 用Stata14跑一直出错，只能13跑)
```

## Connection

### Stata & Mplus 联用调用
 -- dta格式转Mplus的dat格式：
stata2mplus using c:\data\hsb2.dta  // this creates c:\data\hsb2.dat and c:\data\hsb2.inp 
 -- Stata中运行Mplus
help runmplus  // see:  http://www.lvmworkshop.org/home/runmplus-stuff


### epidata文件导出
- 6数据导出 -> Stata格式 -> 选择.rec文件

### R/Python
- 使用python语言在Stata中：
python
exit()
shellout python_plugin.pdf //详细介绍

- Stata-R语言联用,方法一: "rsource",我做SP问卷输出和数据处理采用的
```r
	doedit "D:\aa\book_Stata_Latex_R\Stata_R_SP.do"
	已经修改profile.do文件, R-Stata联用设置 这样就可以在Stata中写R代码 调用R的功能
	global Rterm_path `"D:\R\R-4.0.3beta\bin\x64\Rterm.exe"'   //specify them in order to call R in Stata 
	global Rterm_options `"--vanilla"'
	//例子
	rsource, terminator(END_OF_R)
        library(foreign);
        rauto<-read.dta("myauto.dta", convert.f=TRUE);
        rauto;
        attributes(rauto);
        q();
    END_OF_R
```

方法二："rcall" 比rsource强的是有interactive模式 
```r
	net install Rcall, replace from("d:\rcall")
	help rcall

	library("foreign", lib.loc="D:/R/R-Portable/App/R-Portable/library")
	data<-as.data.frame(med$alternatives$alt.1)   # med是list类型的对象, alt.1是attributes组合的对象
	write.dta(data,"d:/test.dta")
	* R里面的表格tab_BWS_aggregate_score.csv 输出，再用Stata转成Latex表
	rcall:data<-as.data.frame(scores$aggregate)   # med是list类型的对象, alt.1是attributes组合的对象
	rcall:write.csv(data,"d:/tab_BWS_aggregate_score.csv")  # 这个表待会读入Stata，然后用mata转成Latex表
	insheet using "D:\aa\CLDS\BMI\clds2016_m.csv", clear  	
	import delim using "d:\tab_BWS_aggregate_score.csv"  , clear
	texsave _all using "d:\tab_BWS_aggregate_score.tex" , frag replace  ///
			title(Aggregated best-worst scores)  ///
			footnote(Note: The number of respondents is xxx)  
	***例子：综合控制法
	rcall:data(gsynth)
	rcall:names(turnout)

	rcall:set.seed(123456)
	rcall:turnout.ub <- turnout[-c(which(turnout$abb=="WY")[1:15], sample(1:nrow(turnout),50,replace=FALSE)),]
							 
	rcall:out <- gsynth(turnout ~ policy_edr + policy_mail_in + policy_motor, ///
				  data = turnout.ub,  index = c("abb","year"), se = TRUE, inference = "parametric", r = c(0, 5),  ///
				  CV = TRUE, force = "two-way", parallel = TRUE, min.T0 = 8,  nboots = 1000, seed = 02139)

	rcall:print(out)

	rename var1 t
	tsset t
	twoway tsline att cilower ciupper,xline(0) yline(0) scheme(s2mono)

	rcall:out.mc <- gsynth(turnout ~ policy_edr + policy_mail_in + policy_motor, min.T0 = 8, ///
				  data = turnout.ub,  index = c("abb","year"), estimator = "mc", ///
				  se = TRUE, nboots = 1000, seed = 02139)
				  
	rcall:plot(out.mc, main = "Estimated ATT (MC)")
	rcall:print(out.mc)

	rename var1 t
	tsset t
	twoway tsline att cilower ciupper,xline(0) yline(0) scheme(s2mono)
```

方法三：在R语言中run Stata
library("RStata") 

### DOS/shell/web
```r
shell erase do_file.do  //shell (等价"!") allows you to send commands to your operating system
shell                   // or to enter DOS界面
!erase      myfile.txt  // DOS命令erase/del一样
!del        myfile.tex
!rename try15.dta   final.dta

!rmdir D:\aa\manydoc\bak.stunicode  /s /q   //多运行一遍, 删除整个文件夹

winexec notepad "d:\myfile.txt"   //Stata命令 allows you to open file by using notepad
```

- Stata以keyword为字符变量，逐个代入网页，拷贝网页内容 
```r
levelsof keyword,local(levels)
foreach c of local levels {
    copy "http://www.baidu.com/s?wd=`c'" "D:\temp.txt",replace
    infix strL v 1-200000 using "D:\temp.txt",clear
    keep if index(v," 百度为您找到相关结果约")
    replace v = ustrregexs(1) if ustrregexm(v," 约(.+?)个")
    local date = c(current_date)
    local time = c(current_time)
    post baidu ("`date'") ("`time'")("`c'") (v[1])
}
postclose baidu
use"D:\baidu.dta",clear
replace baidu = ustrregexra(baidu,",","")
destring baidu,replace
```

### Latex/Markdown

- 放弃Stata动态文件编写(stata14终于能完美支持ASCII中文了): markdoc, ketchup

- Stata输出eps图，插入Latex，再编译
```r
	cd d:\
	sysuse auto
* run reg
	eststo mytab: reg mpg weight length
	esttab mytab using "mytable.tex", style(tex) replace
* draw graph.eps, turn to .pdf
	scatter mpg length
	graph export mygraph.eps, replace
	!epstopdf mygraph.eps  // 将eps图片转pdf,适合在latex中插入
* compile pdf by latex
	!pdflatex myoutput.tex -no-shell-escape   /* epstopdf和pdflatex都是MiTex的功能
                                                   shell-escape是pdflatex的一个选项 */
	!pdflatex  CTang_CV.tex  -output-directory=.\output\    //定向编译
	!copy .\output\CTang_CV.pdf      .\             //再copy pdf到本文件夹

% Latex file myoutput.tex
\documentclass[11pt]{article}
\usepackage{graphicx}

\begin{document}

\section{My table}
\input{mytable.tex}

\section{My graph}
\includegraphics{mygraph.pdf}

\end{document}
````

- Stata回归、制table，插入Latex
```r
program main
    prepare_data
    run_regressions
    output_table
end 

program prepare_data
    use ../external/tv_potato.dta, clear 
    xtset countycode year 
    label variable log_chip_sales "Log Chip Sales" 
    label variable tv_linear "TV"
end 

program run_regressions  // 调用下面的 program setup_table
    local dep_var   "log_chip_sales"
    local tv_vars   "tv_linear"
    local vce       "vce(cluster countycode)"
    
    eststo: reg `dep_var' `tv_vars', `vce'
    setup_table, unitvar(countycode) timevar(year)
    
    eststo: xtreg `dep_var' `tv_vars', `vce'
    setup_table, unitvar(countycode) timevar(year)
    
    eststo: reg `dep_var' `tv_vars' i.year, `vce'
    setup_table, unitvar(countycode) timevar(year)
    
    eststo: xtreg `dep_var' `tv_vars' i.year, `vce'
    setup_table, unitvar(countycode) timevar(year)
end

program setup_table   // 被上面的program run_regressions调用
    syntax, unitvar(varname) timevar(varname) 
		if "`e(ivar)'" == "`unitvar'" {
			estadd local unit_fe "Yes"
		}
		else {
			estadd local unit_fe "No"
		}
    local cmdline "`e(cmdline)'"
    local cmd_substr : subinstr local cmdline "i.`timevar'" "", count(local cmd_time_fe) 
		if `cmd_time_fe' == 1 {
			estadd local time_fe "Yes"
		}
		else {
			estadd local time_fe "No"
		}
end 

program output_table 
    esttab using ../output/regressions.tex, ///
        replace drop(*.year) ///
        obslast se label nonotes nomtitles collabels(none) ///
        cells(b(star fmt(4)) se(par fmt(4)))  ///
        scalars("unit_fe County Fixed Effects" "time_fe Year Fixed Effects") ///
        mgroups("Log of Chip Sales", pattern(1) prefix(\multicolumn{@span}{c}{) ///
            suffix(}) span erepeat(\cmidrule(lr){@span})) 
end 

main  // 启动执行
```

- Table - r1_mandatory and r2_mandatory's before-after mean comparison - by countries
```r
preserve
     * sample selection 
 drop if Country=="" | r1==.  // r1 和 r2 的missing obs完全一样
     * T-test and mean comparison
 encode Country, gen(country)  // string → numerical, so can be used by next ttab
	 foreach v in 1 2 {
	 
		 ttab r`v'_mandatory, by(post) over(country)      // calculate difference
		 mat r=r(coefs)
		 ttab r`v'_mandatory, by(post) // for total
		 mat t=r(coefs)

		 ttab r`v'_mandatory, by(post) over(country) tshow // calculate t-stat
		 mat rr=r(coefs)
		 ttab r`v'_mandatory, by(post) tshow
		 mat tt=r(coefs)

		 tabstat r`v'_mandatory , by(country) s(mean count) save  // calculate mean and observations
		 tabstatmat A

		 mat rr=[r\rr]
		 mat tt=[t\tt]
		 mat rr=[rr,tt]
		 mat rr=rr'
		 mata: st_matrix("rr" , select(st_matrix("rr"),(1,1,1,0,0,1))) //2个mata函数，2个功能
		 mat out`v'=[rr, A] 
		 
	   }
  mat out=[out1 , out2]
  mat colnames out = "Before IFRS" "After IFRS" Diff T-stat Total N  "Before IFRS" "After IFRS" Diff T-stat Total N 
  mat rownames out = AUSTRALIA AUSTRIA BELGIUM CANADA DENMARK FINLAND FRANCE GERMANY GREECE "HONG KONG" INDIA INDONESIA IRELAND ITALY JAPAN KOREA(SOUTH) MALAYSIA NETHERLANDS NORWAY PAKISTAN PHILIPPINES PORTUGAL SINGAPORE "SOUTH AFRICA" SPAIN SWEDEN SWITZERLAND TAIWAN THAILAND "UNITED KINGDOM" "UNITED STATES" Total
     * 格式化out矩阵，加star
  local bc = rowsof(out)
  mat star = J(`bc' , 12 , 0)
  foreach v in 4 10  {
   forvalues k = 1/`bc' {
	  if abs(out[`k' , `v']) <= 1.645  {
	  mat star[`k' , `v'] = 0
	  }
	   else if (abs(out[`k' , `v']) > 1.645 & abs(out[`k' , `v']) <= 1.96 ){
	  mat star[`k' , `v'] = 1
	  }
	   else if (abs(out[`k' , `v']) > 1.96  & abs(out[`k' , `v']) <= 2.58 ){
	  mat star[`k' , `v'] = 2
	  }
	   else if abs(out[`k' , `v']) > 2.58 {
	  mat star[`k' , `v'] = 3
	  }
	  }
	  }
     * output word
  frmttable using output /*in default word */, statmat(out) annotate(star) asymbol(*,**,***) sdec(2,2,2,2,2,0,2,2,2,2,2,0 ) landscape a4  //tex option写成latex文件
restore
```

## Data/Big data

- 随机数
```r
set seed ####   //给定初始数值，相同的初始数值生成的伪随机数列完全一致。
runiform() or rnormal(m, s) //随机正态, uniform() is for Stata 10
help rnd       //random number generators -- 包括各种分布
```

- 节省行数
```r
#delimit; 
gen lnpergas=ln(pergas);gen lngasp=ln(gasp);
#delimit cr
```

- local/macro用法
```r
macro list       // 查看
macro drop _all  // 清除

local a: display %6.2f ln(10)  // local a: 后接命令， 类似 bysort hos: 

local cs 1  //while的loop好处是结果最终一起显示
while `cs' == 1 {
replace xxx省略xxx
local cf=0      // 归0，否则会无限循环，或者用exit
}
所以还是用 if 更好
if   `cs' ==1 {
}
```

- 查看返回结果:
```r
`return list      //看r()
ereturn list     //看e()
r()中的数据导入矩阵：
mat ic=nullmat(ic) \ r(wtp)
mat ic=nullmat(ic) \ r(wtp)   //column-join (\) operators 可向下连续扩展矩阵
                                row-joinn 逗号 ，
egen max=max(groupid_total)  //提取var的最大值
```

- 区分
1. <缺失值>用.表示,比任何自然数都大，所以>60会包括缺失值, 所以一定要限制上限!
1. <空字符>是one string missing value，程序中用""代替

- 区分 "/" 和 "-" 的用法
1. 一个变量内数字用1/7250，
1. 一个变量到另一个变量用pro1-pro7

- 区分
1. tab和table命令的输出格式很不一样！
1. tab type if num==1, sum(pro1)  //按分类求均值，如9种卫生机构，各自的pro1的均值

- 区分Type和Format：
1. Format指显示格式，并不改变数据大小。（如日期也是以数字存储的，可用format改成日期，而且日期变量赋值给别的变量会变成数字，因为没有指定format）
1. Type指数据存储格式, e.g.如计算年龄age：gen age=(mdy(12,1,2009)-bir)/365.25，mdy是日期格式吗？


- Stata 14 使用了 Unicode（统一码、万国码），之前的dofile只识别UTF-8编码，以前版本dofile都需要convert to UTF-8 by Notepad++。Stata 14 数据库打开时会乱码，转码如下：
```r
cd d:\对应目录
unicode analyze *.do  //或者 .dta数据文件
unicode encoding set gb18030
unicode translate *.do
```

- help getsymbols //API data from Quandl, Google Finance, Yahoo Finance, and Alpha Vantage

### 数据预处理

- 单纯转置stata数据库（非矩阵）
help xpose  
help sxpose //Transpose of string variable dataset

- 填充/替代/

recode alt (1=1)(2=0), gen(ASC)  //赋值神令,不用gen和replace；赋值规则是 gen ASC=0 if alt==2  |  ASC=1 if alt==1

```r
rename _all, lower  //变量名称全部是英文小写

tsset Year    //序列填充
tsappend , add(21)

carryforward  //分组填充
xfill
bysort id: replace number=number[1]    //组内第一个替代每一个

同一个变量前一个值赋给下一个：
forvalues var=1/7250 {
replace var=var[_n-1]  if var==.
}
drop if _n==_N //drop the last obs

复制数据
expand 2 if ...     // if范围内obs翻2倍
expand n , gen(id)  // 所有obs翻n倍，同时产生id区分新旧obs

截取
gen y=mod(x,10)  //截取数字的个位数, 同理可退后二位,三位,四位数
winsor & truncate   缩尾 & 截尾;  缩尾e.g.ln(1) → ln(1.01) 避免ln(1)=0
replace x=1.01      //目的是将离群值变成1%或99%分位值
```

- loop over all the observations
```r
   gen y=.
   forvalues i=1/`=_N' {
   replace y=x[`i'] if _n==`i'
       }
you'll get the exact same result far more quickly and easily with:
   gen y=x  
```

- Stata命令前缀prefix, 如产生交互
```r
i.  to specify indicators for each level (category) of the variable
c.  to interact a continuous variable with a factor variable
L.  lag  x_t-1
F.  lead x_t+1
D.  diff (x_t)-(x_t-1)
S.  diff (x_t)-(x_t-1) (seasonal)

help fvvarlist  //interaction–indicators交互项 factor variable，直接full factorial
```

- 筛选
```r
有了inlist，你还在一直用if吗？
if语句：    gen x=1 if rep78==1|rep==3|rep==4|rep==5
inlist函数：gen x=1 if inlist(rep78,1,3,4,5)
            gen y=1 if inlist(make,"AMC Concord","AMC Pacer","Buick","Ford")
            keep if inlist(icd10,"O80.901","O82.901","700001","O80.001","H25.901","H26.901")  
```
   
### 字符

subinstr(s1,s2,s3,n)函数的应用--在s1中剔除s2，依次替换成n个s3，e.g.
replace hos=subinstr(hos,char(10),"",.)
char(10)就是这个可恶的ASCII为10的Line Feed符号
另有妙用：
	count if length(subinstr(pattern, "1", "", .))==1  //Counting substrings within strings


gen x= y + z  //合并字符
replace ctown = subinstr(ctown, "meizhou",  "putian", 1) // 替换变量ctown中的 meizhou 为 putian.

tostring  
destring v17, gen(vv17) force   // 如果没有异常则实施强制转换

fdta _all , from("AMC") to("SUV")  // 专门替代(字符变量)字符的命令，比subinstr()函数好用太多
fdta _all , from("AMC")  // 替代性删除

substr(s,2,4)    //截取字符函数, 从s的第2个字符开始截取4个字符
replace town=subinstr(town, "*" , "" , .)    //修改字符变量,删除字符中的星号* 或空格

字符变量转数值变量
encode hosid , gen(hid) //encode可以完美的将字符型的factor变量 转换成 数值型的factor变量，而且带 label
decode  //vise verse

- 解决--csv含20位ID和中文字符，后缀1,2,4表示第1,2,4个变量 字符型导入。
  PS：Stata无法加载16位以上数字digits并保持精度(会损失尾数精度)，这种情况只能以字符型处理
import delimited D:\aa\桌面\aa.csv, stringc(1,2,4) encoding(gb2312) clear 
import excel using "$path\temp_tab.xlsx", clear sheet("医学专业招生在校毕业数52-2014") cellrange(:o45) sheet("SALC")

```r
replace xxx= strlower(xxx)  //字符串变小写, 注意：replace命令在使用时，如果赋值和原值相等，则不会计算在（？real changes made） 今天在计算1397个医院的人数时，结果只替代了1356个，就发现原来有41个医院前后人数相同。

从字符串最后面开始删掉两个字符:
  replace county=reverse(county)
  replace county=reverse(substr(county,3,8))
  
replace v=1  if regexm(id,"福建省")  //检查<字符串变量>是否含所需字符
replace v=3  if regexm(id,"县") | regexm(id,"区")

destring var, replace   //字符转数字出现问题
出現"var contains nonnumeric characters"
解决办法:
gen byte notnumeric=real(var)==.    //real()是函数,里面放出问题的变量名
tab notnumeric
br if notnumeric==1                          //==1 where nonnumeric characters


字符中的dot .
count if regexm(pattern, "\.")  不能用 count if regexm(pattern, ".")
通配符 regexm(str, "[0-9]+\.[0-9]*[%]$") 
```

### Time/age

if year==1960     //don't do this!
if year[1]==1960  // right, first observation is 1960

```r
drop if date2<date("01jan2010","DMY") | date2>date("30sep2011","DMY") //删除时间段

Age：算age（从单纯的年、月两个变量）
	gen birthdate=1
	gen bir=mdy(birthmonth, birthdate, birthyear) // 用原始birthmonth，缺失值少
	format bir %td
	gen age=(mdy(9,1,2014)-bir)/365.25
	drop birthdate bir	

日期: 设变量x是字符型，且格式是"1-aug-02"
monthly(s1,s2[,Y])
date(s1,s2[,Y])

g m = monthly(actual_date, "MY")  //这里只能gen,不能replace,因为type mismatch
format m  %tmNN/CCYY

g d = date(x,"DM20Y")
form d  %td

//extract month and year component from a variable (dm) with %tm format（e.g.1953m1）
gen date = dofm(dm)
format date %d
gen month=month(date)
gen yr=year(date)

//一步到位牛逼的直接命令 https://blog.stata.com/2015/12/17/a-tour-of-datetime-in-stata-i/
help todate
help anythingtodate 

anythingtodate cysj ,k
 //出院时间
hist cysjc12 if cysjc12>date("2015-01-01", "YMD")

// YYYYMM date to "ym" Stata
gen ndate = 201606
gen sdate = "201606"
gen ndate2 = ym(floor(ndate/100), mod(ndate, 100))
gen sdate2 = ym(real(substr(sdate, 1, 4)), real(substr(sdate, -2,2)))
format *2 %tm
l
     +-----------------------------------+
     |  ndate    sdate   ndate2   sdate2 |
     |-----------------------------------|
  1. | 201606   201606   2016m6   2016m6 |
     +-----------------------------------+
// 
	gen  rysj2 = date(string(rysj,"%8.0f"),"YMD")
	format %td rysj2
```

### Stata矩阵和Mata


mat l e(b) //查看系数矩阵,矩阵导出到excel
matlist 比 matrix list 更方便display矩阵

- matrix -> variables 
```r
svmat      //
svmat2 A , rnames(ym) full //可把所有rownames save into a variable
mkmat  // variables -> matrix
xsvmat
```

- 将统计频数转入mat，再mat转variables，再画图
```r
tab  edu urban, matcell(aa)  // 频数直接存入矩阵aa,此命令不会抹掉existing data，而是自动列在后面
svmat aa
gen n=_n in 1/6
label define edu2 1"High_school" 2"SVD" 3"College" 4"Bachelor" 5"Master" 6"PhD"
label values n edu2
graph bar aa1 aa2 , over(n) ytitle(Number of physicians) b1title(Education) legend(label(1 Rural hospitals) label(2 Urban hospitals))

	svmat A  //矩阵画图-连线图
	drop in 24
	gen n=_n
	graph twoway connected A1 n
```

- Speaking Stata between tables and graphs
  - tabout--属于Stata自带基本命令，是基本统计量表格输出的好命令
  - tabstat v11-v16, by(v3) s(mean) //tabstat妙用--做多个变量按同一分组统计均数
```r
help taboutgraph  //a wrapper for tabout, plot output of two-way tab function
taboutgraph var1 var2 [aw=weight] using "filename_to_savedatato.csv", ///
  gc("graph bar") ta(cells(col) f(2 2 2 2)) replace go( note("Source: XXX") ///
	b1title("Quintile") title(`"Composition of Population"') ytitle("Percent of population in the quntile"))
```

- 用统计table画图命令 collapse/contract：Make dataset of summary/freq statistics这个命令太好了
```r
preserve
  collapse draft, by(timing year) //Make dataset of summary statistics
  gen ldraft=log(draft)
  twoway(connect ldraft year if timing==1955, lcolor(black)) ///
	(connect ldraft year if timing==1956,msymbol(diamond) lcolor(pink) lpattern(dash)), legend(off) ///
	text(10.49 1955.5 "Collectivization in 1955 (569 counties)", color(black)) ///
	text(10.58 1955.7 "Collectivization in 1956 (1031 counties)", color(pink)) ///
	xline(1954, lcolor(black)) xline(1955, lcolor(pink) lpattern(dash)) ///
	xtitle("year") ytitle("log(number of draft animals)")
restore
```

- 互换矩阵和descriptive statistcs
```r
gen a=1  //为了两年间数据均衡，删掉某一年没数据的医院
bysort hos: gen pn=sum(a)
bysort hos: egen b=max(pn)
drop if b<10
drop if regexm(hos,"漳平市妇幼保健院")
replace hos=subinstr(hos,char(10),"",.) //去除hos字符断行问题
encode hos, generate(hos1) //encode是为了下句mean的over选项

方法一 倒数-> 哈氏乘法
label drop hos1
quietly: mean lfdc if year==2010,over(hos1) //over只能用numeric变量
mat lfdc1=e(b)
quietly: mean lfdc if year==2011,over(hos1)
mat lfdc2=e(b)
quietly:mean drugout if year==2010,over(hos1)
mat drug1=e(b)
quietly:mean drugout if year==2011,over(hos1)
mat drug2=e(b)

mat xx=[lfdc1\lfdc2]  // 右下斜杠是分行，逗号是并列。
mat yy=(drug1\drug2)

local aa=rowsof(yy)      //本loop将矩阵各值取倒数
local bb=colsof(yy)
forval x=1/`aa' {
forval y=1/`bb' {
mat yy[`x',`y']=1/yy[`x',`y']
}
}
mat ld=hadamard(xx,yy)   //哈氏乘法--元素*元素，且乘以倒数==相除
                       
local cn=e(over_labels)  //问题2，引用e(over_labels)赋值给ld 
mat rownames ld=2010 2011 
mat colnames ld="`cn'"
mat ldt=ld'
mat l ldt
local  : rownames ld

方法二  tabstatmat  
tabstat lfdc drugout if year==2010, by(hos) s(mean) save //save选项将结果存入r()
tabstatmat A  //关键命令
tabstat lfdc drugout if year==2011, by(hos) s(mean) save //save选项将结果存入r()
tabstatmat B

local cc=rowsof(A)
forval x=1/`cc'{
mat A[`x',1] = A[`x',1]/A[`x',2]
mat A[`x',2] = B[`x',1]/B[`x',2]
}
mat colnames A=2010 2011
mat l A

方法三 statsmat //Produce matrix of descriptive statistics
   foreach x in 1 2 3{  // 先按三个病种Loop
     foreach v  of varlist day fee feeday drug drugday{  // 再每个病种的变量
	    statsmat `v' if case==`x', by(own) mat(`v'`x') s(mean sd) f(%3.2f) xpose
        }
		}
```

- Stata矩阵区别于mata矩阵，得互相调用
```r
st_view() make Mata matrices "x" from a Stata dataset "varname"
st_data() Load copy of current Stata dataset
st_numscalar() obtain value from colsum(x) and put into r(sum)
    // mata里面用Stata的矩阵切记加双引号""
 mata: st_matrix("rr" , select(st_matrix("rr"),(1,1,1,0,0,1))) //3个mata函数，3个功能，st_matrix()将stata中的matrix转给mata
 mata: st_matrix("temp"):/st_matrix("xiaono")  //element-by-element相除， 相乘用 :*
 mata: st_matrix("JJ", st_matrix("aa"):/st_matrix("minorno"))  //JJ是Stata矩阵不是mata矩阵
 
mata矩阵A，直接type矩阵名就list矩阵（不像Stata矩阵需要mat list命令）
A
```

方式一
>mata: istmt   //for programmers,单行不要end
>mata: Z[., .]=X:-mean(X)  //每个ob减mean(X)就是center a variable on the mean

方式二
>mata          //if an error occurs, still stay in mata
>    istmt   
>end  

Mata最大优点：可任意自编函数 → 问题来了：很多函数如何储存？  
help mata mlib //a group of functions, like library

```r
help max()      //两命令不同
help mata max()
mata query  // mata setting
mata describe  //最常用两mata命令
mata drop
mata clear

viewsource diag.mata
help moremata  //see extra mata function, especially their source code
```




### Big data
[超大数据](https://www.cpc.unc.edu/research/tools/data_analysis/statatutorial/misc/large_files
)-电脑内存利用超97%，首先进去drop变量
describe using "bigfile.dta"  //不加载bigfile进内存
lookfor and lookfor_all
use list_of_variables using
use in
use if
use if inrange(runiform(),0,.1) using "bigfile.dta"

help ftools  //Stata处理大数据的新套件  
help gtools

help parallel  //stata并行处理多个文件
