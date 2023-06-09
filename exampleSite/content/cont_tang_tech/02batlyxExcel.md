---
title: bat Excel/Word Lyx/Latex
weight: 20
---


## bat 

A batch file(.bat 批处理文件) is a script file in DOS, OS/2 and Microsoft Windows. It consists of a series of commands to be executed by the command-line interpreter, stored in a plain text file.

- build a new project

1. 先建立project文件夹
1. 到D:\desk\tools\newproject里面跑1_buildfolder.bat

- 模板
```r
SET log=.\output\rundirectory.log

:: Clear output and externals 
DEL /F /Q .\output\     :: del命令只能删文件，rd命令删文件夹. 
rd Filemon              :: 删除文件夹。 /q参数表示，删除不需要确认。 /s参数表示删除该文件夹及其下面的子目录和文件
                        :: filemon文件夹和该bat文件在同一目录下，就省去具体路径
RMDIR .\external /S /Q
MKDIR .\external

:: Log start 
ECHO rundirectory.bat started > %log%
ECHO %DATE% >> %log%
ECHO %TIME% >> %log%
DIR .\output\ >> %log%

:: Get externals
SET rev=4
SET svnpath=https://booth.svn.cloudforge.com/svndemo/trunk/
SVN export "%svnpath%/analysis/output/regressions.tex@%rev%" "./external/regressions.tex" --force -r%rev% >> %log%
SVN export "%svnpath%/raw/data/potato.dta@%rev%" "../external/potato.dta" --force -r%rev% >> %log%
SVN export "%svnpath%/raw/data/tv.dta@%rev%" "../external/tv.dta" --force -r%rev% >> %log%

:: Compile the paper
PDFLATEX .\source\tv_potato_submission.tex -output-directory=.\output\ >> %log%

:: Delete auxiliary files 
DEL .\output\tv_potato_submission.aux .\output\tv_potato_submission.log 

:: Merge files, clean resulting data set 
"D:\stata13_SE_unsw\StataSE-64" /e do mergefiles.do 
"D:\stata13_SE_unsw\StataSE-64" /e do cleandata.do

:: Run regressions 
"D:\stata13_SE_unsw\StataSE-64" /e do "D:\svndemo\branches\analysis (alternative regressions)\code\regressions.do"

:: Append log files 
COPY %LOG% + regressions.log %LOG%
DEL regressions.log

:: Log end 
ECHO rundirectory.bat completed >> %log%
ECHO %DATE% >> %log%
ECHO %TIME% >> %log%
```

- bat重命名文件
```r
ren aaa.txt bbb.doc   ::将 aaa.txt 命名为 bbb.doc
ren [Drive:][Path] filename1   filename2
```
- 复制cd.dll文件至windows\system32

copy cd.dll %windir%\system32   
 
- 目录方式打开文件夹

start explorer "D:\desk\Dropbox\IFRS and Inst"

- 某个程序打开某个文件

start D:\stata13_SE_unsw\StataSE-64.exe   "D:\desk\Dropbox\IFRS and Inst\0prog_IFRS.do"

- 打开任何文件-如noteexpress

start "" "D:\desk\tools\英文.nel"

- 添加注释--段首，段后

start "" "D:\desk\LITERATURE\POOL-2014\03090561111137697.pdf"   ::打开pdf


## Excel/Word 

- 取消Word文档中所有超链接: 先Ctrl+A(全选)；然后，Ctrl+Shift+F9(取消超级链接)。

- Excel以下内容可转至word, 再用通配符替换:

- WORD中的通配符主要有：  
*代表0至任意个字符；  
^?代表任意字符；  
^#代表任意数字；  
^$代表任意字母；  
^&代表查找内容（在替换框使用，相当于原有内容基础上附加内容）。

选择所有汉字， 在“查找内容”输入 [!^1-^127]

- 产生随机数  
=RANDBETWEEN(0,1)  //随机产生0或者1，binary
=round(rand()，2） //随机产生0-1之间任意小数

- excel中换行？？  alt+回车

- excel快速下拉填充, 双击填充格右下角黑色十字标记

- 加总  
=SUM(1/COUNTIF(D2:D477,D2:D477))
按 ctrl+shift+enter 结束输入

- excel 批量插入空行 的方法：
用alt+F11进入VBE编程，双击左边你的工作表名称，复制以下代码：
```
Sub 添加4空行()
Do While ActiveCell <> ""
    ActiveCell.Offset(1, 0).Resize(4, 1).EntireRow.Insert shift:=xldowm
    ActiveCell.Offset(5, 0).Select
Loop
End Sub
```
Alt+Q返回Excel, 将光标停在需要插行的下一行按Alt+F8插入，遇到空单元格会停止插入。  
注：这个操作无法撤消; 若想加更多行修改(4,1)和(5,0)数据

- 只去除全部汉字：全部替换[!^1-^127]，且高级→通配符

- 在年月日中间插入空格如：20090805成为2009 08 05
方法：=REPLACE(REPLACE(A1,5,," "),8,," ")
      =TEXT(A1,"0000 00 00")

- EXCEL去掉原来表格中的函数----复制→右键，选择性粘贴，数字格式

- 格式选定的“自定义”中可以显示“年月yyyy-mm”

- excel中即使没有19也会默认为19xx开头的年份，这点很好，省的加19

- excel中批量去除空行：编辑→定位→空值→删除-整行

- 各种符号

Excel中“&”这个符号表示“与”

数据计算  ABS 求出参数的绝对值。 

条件判断  AND “与”运算，返回逻辑值，仅当有参数的结果均为逻辑“真（TRUE）”时返回逻辑“真（TRUE）”，反之返回逻辑

条件判断 “假（FALSE）”。 

数据计算  AVERAGE 求出所有参数的算术平均值。 

显示位置  COLUMN 显示所引用单元格的列标号值。 

字符合并  CONCATENATE 将多个字符文本或单元格中的数据连接在一起，显示在一个单元格中。 

条件统计  COUNTIF 统计某个单元格区域中符合指定条件的单元格数目。 

显示日期  DATE 给出指定数值的日期。 

计算天数  DATEDIF 计算返回两个日期参数的差值。 

计算天数  DAY 计算参数中指定日期或引用单元格中的日期天数。 

  DCOUNT 返回数据库或列表的列中满足指定条件并且包含数字的单元格数目。 条件统计

  FREQUENCY 以一列垂直数组返回某个区域中数据的频率分布。 概率计算

  IF 根据对指定条件的逻辑判断的真假结果，返回相对应条件触发的计算结果。 条件计算

  INDEX 返回列表或数组中的元素值，此元素由行序号和列序号的索引值进行确定。 数据定位

  INT 将数值向下取整为最接近的整数。 数据计算

  ISERROR 用于测试函数式返回的数值是否有错。如果有错，该函数返回TRUE，反之返回FALSE。 逻辑判断

  LEFT 从一个文本字符串的第一个字符开始，截取指定数目的字符。 截取数据 Left(A1, 2)

  LEN 统计文本字符串中字符数目。 字符统计

  MATCH 返回在指定方式下与指定数值匹配的数组中元素的相应位置。 匹配位置

  MAX 求出一组数中的最大值。 数据计算

  MID 从一个文本字符串的指定位置开始，截取指定数目的字符。 字符截取

  MIN 求出一组数中的最小值。 数据计算

  MOD 求出两数相除的余数。 数据计算

  MONTH 求出指定日期或引用单元格中的日期的月份。 日期计算

  NOW 给出当前系统日期和时间。 显示日期时间

  OR 仅当所有参数值均为逻辑“假（FALSE）”时返回结果逻辑“假（FALSE）”，否则都返回逻辑“真（TRUE）”。 逻辑判断

  RANK 返回某一数值在一列数值中的相对于其他数值的排位。 数据排序

  RIGHT 从一个文本字符串的最后一个字符开始，截取指定数目的字符。 字符截取

  SUBTOTAL 返回列表或数据库中的分类汇总。 分类汇总

  SUM 求出一组数值的和。 数据计算

  SUMIF 计算符合指定条件的单元格区域内的数值和。 条件数据计算

  TEXT 根据指定的数值格式将相应的数字转换为文本形式 数值文本转换

  TODAY 给出系统日期 显示日期

  VALUE 将一个代表数值的文本型字符串转换为数值型。 文本数值转换

  VLOOKUP 在数据表的首列查找指定的数值，并由此返回数据表当前行中指定列处的数值 条件定位

  WEEKDAY 给出指定日期的对应的星期数。 星期计算 






## Lyx/Latex
### Lyx

- 多行注释 iffalse ... fi
iffalse
多
行
fi

- Lyx在article（即英文模板）使用中文方法：  
在setting→Document Class→Class Options中的Custom一栏填入UTF8（这个选项将传送给ctex宏包）, 还要把语言的encoding设置为Unicode(XeTeX) (utf8)  

- Lyx编译： ctrl+D or ctrl+R
 
- 在LaTeX Preamble中输入如下LaTeX 代码  
\\usepackage{xeCJK}  
\\setCJKmainfont{SimSun}  
只需点击View>View[pdf(XeTeX)]即ctrl+R可完成编译

- 每一次新建文档，首先要选择文档的类型，这里以article的类型举例:  
    1. 点击 File > New ，新建一个空白文档， 点击 Document> Settings
    1. 分别点击左侧的导航栏，在Document class中选择 CTeX，这是由于我们的文档中包含中文，参考文献， 目录的格式，CTeX类型的文档能给予很好的支持。  
    3. Language中选 Chinese (simplefied)，Encoding 选 Unicode (XeTeX) (utf8) 这是由于我们在后续编译LyX文档的时候， 要用到XeTeX程序，而要在正文中使用中文，xeCJK宏包的效果很好。  
    4. 在output设置Default output format 为 PDF (XeTeX)，我们用XeTeX编译LyX文档， 生成PDF文档。
   
### Latex

- pdfpages宏包插入文件无页码的解决 

在\\includepdf命令之前加入\\includepdfset{pagecommand={\thispagestyle{plain}}}，就可以对后面的所有include加页码。

- 文章中长横线 $-$   %即数学模式的减号

- \\item
\\item[]  % 这样就没有前面一横


- 插入pdf图片代码：
```r
  \begin{figure}[htbp!] 
	\centering    
	\includegraphics[width=12cm]{./output/Pat_see_Doc_on_expected_profit.pdf}\\
	\caption{Patients' decision to see Doctor (get diagnosis) on expected profit}  \label{fig2}
	\end{figure}
```

- 多人合作修改latex文档：
一是导言区加 \\usepackage[inline]{trackchanges}   和  \\addeditor{Tang}; 
二是在latex文中加 \\note[Marian]{....}


- latex-caption-加上标，不用 ^, 而用
\\textsuperscript{1}

- 主latex文件夹下output文件夹插入：
\\input{./output/filename.tex}

- 正文中插入数学符号需要数学环境:  `\(\theta\)` or `$\theta$`

- online latex:  sharelatex .vs. writelatex
最后决定用writelatex，  google account登录, 主要是其模板很好，可下载了自己单机用

- 即使是bib参考文献里面, `&` 不能单独用, 必须是 `\&`, e.g. `Journal of Economic Behavior \& Organization`

- 表格 \\label 放在\\caption 后面 , 引用的时候才不会错

- latex文件分割插入
```r
\input{}   不使用new page
\include{}   使用new page
插入pdf
\usepackage{pdfpages}
\includepdf[pages=-]{myfile.pdf}
```

- latex表格实验简介模板：
```r
\documentclass[a4paper] {article}
\usepackage{booktabs}  % \toprule
\usepackage{multirow}  % 跨行
\usepackage{array}     % 设定cell width
\usepackage{pdflscape} % \begin{landscape}
\usepackage{longtable}
\usepackage{caption}  % longtable caption center 
```

- latex表格内划线：
```r
\toprule % 划线同\hline
\midrule
\cmidrule{2-9}  % 划线2-9columns
\bottomrule  %这套才是专用表格划线--上下粗，中间细

\hline %hline划线比较难看
\chline{2-9} 
```

- latex表格cells  
```r
latex表格cells内左对齐：  
\multicolumn{1}{l}{文本内容}   
\multicolumn{'num_cols'}{'alignment'}{'contents'}  \multicolumn{1}{p{2cm}}{CL}  % 它要加p{}
\multirow{''num_rows''}{''width''}{''contents''}      \multirow{2}{2cm}{CL}     % 它不加

latex表格cells内的摆中间-最好用空格调整    \hspace{5cm}  
latex表格cells内的 上标  $^1$
 
latex表格cells内的 _ 号必须使用 \_
latex表格cells内的 > 号必须使用 \textgreater
latex表格cells内的 < 号必须使用 \textless
latex表格cells内的 = 号直接使用
```

- 英文双引号--分别敲1左边键两次, 回车左边键两次: ``AAAA''

- 引用 `caption{} \label{ch:xx}  % ch=chapter` 即本chapter内引用，适用于PhD论文
Figure \\ref{ch:xxx}

- 参考文献引用： （正文中自己写，或者word和noteexpress联用产生\\cite{xxx}）
```r
\newpage
\nocite{CurrieRossin-1756, AlmondCurrie-1194, CurrieStabile}  %隐藏引用，引用从NoteExpress复制
\bibliographystyle{alpha}     %reference按字母顺序
\bibliography{fluo.bib}       %Bibtex文献数据库从NoteExpress导出题录
参考文献citation格式:   \usepackage{natbib}

一定打开格式对照图  http://www.reed.edu/cis/Help/LaTex/images/natbibA-Ncitations.png

常用其实就两种： (\citet*{}, \citep*{} 用于完整名字，但是根本无用)
\citet{}    % text 无括号，会自动识别两个以上作者，并自动缩写
\citep{}   % 括号 (Lim and Yang et al., 2004; Tang and Zhang et al., 2014)
```

```
斜体     \textit{}   或者  {\it xxxx}
粗体     \textbf{}  或者  {\bf xxxx}
下划线 \underline{}
```

- 页面内部调整行距
```r
\usepackage{setspace}

\begin{spacing}{2.0}
\end{spacing}
```

- 多个公式左对齐且无编号
```r
\begin{align*}
& equation 1 \\
& equation 2 
end{align*}

\begin{equation} \label{eq:erl}
a = bq + r
\end{equation}

where \eqref{eq:erl} is true if $a$ and $b$ are integers with $b \neq c$.
```

- 注释有3种:
```r
1. 用%注释一行文字;
2. 用 \iffalse .... \fi 注释一段文字;
3. 用 \begin{comment} ... \end{comment} -- 宏包\usepackage{verbatim}.
```

`$>$`        ＞大于号  
`$\vert$`    竖线

- latex + dvipdfmx 编译的话，eps、pdf、png、jpg 都可以用

- 设置字体大小命令由小到大依次为：
```r
\tiny
\scriptsize
\footnotesize
\small
\normalsize
\large
\Large
\LARGE
\huge
\Huge


横向距离  \hspace{1cm}
纵向距离  \vspace{1cm}

两个quad空格	a \qquad b		两个m的宽度
quad空格	a \quad b		一个m的宽度
大空格	a\ b		1/3m宽度
中等空格	a\;b		2/7m宽度
小空格	a\,b		1/6m宽度
没有空格	ab		
紧贴	a\!b		缩进1/6m宽度
```

- Q: 在Beamer中如果直接用pdflatex编译，则eps格式的图形无法识别，这样，一种方法是将eps转换为pdf格式。对于少量需要转换的图形，这种方法无可厚非，但是若有大量的文件需要转换，则转换调整会花费大量精力。如何直接在beamer中应用eps格式的文档呢？  
A: 若直接插入eps格式到beamer中，需要通过dvi-ps-pdf形式来生成幻灯片。具体做法：
    1. 与其他tex文件一样，首先使用Texify编译源文件,得到dvi文件。
    1. 使用dvi2ps，生成ps文件。
    1. 使用ps2pdf，生成pdf文件，这也是最终需要得到的文件。
