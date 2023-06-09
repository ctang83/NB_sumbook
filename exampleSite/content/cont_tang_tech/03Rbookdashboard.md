---
title: Rbook
weight: 30
---

# R_Onlinebook/Dashboard


### Blogdown/Bookdown

**优点：don't need to build website locally**

build a [personal website (now I treat it as my information hub)](https://ctang.netlify.app/) using a Hugo theme, synchronised on the GitHub, and deployed through netlify (GitHub granted access to the netlify)

1. Static website built on the Blogdown (Hugo, an application .exe) - with multiple template, including personal profile, research group, slides, online course, and project documents(similar to Bookdown) 
1. I also chose VScode that attached to R language
  1. Type `blogdown::serve_site` to locally-preview all website updates
1. How Bookdown works? 
  1. `bookdown::serve_book()` works in the same way to preview
  1. `bookdown::preview_chapter()` in less time-consuming
  1. `bookdown::render_book` build the whole website


### Docker-setting env

For automation/integration, the first step is building an environment.  
1. Historically, virtual machine 虚拟机是最早的带环境安装, 缺点-慢/资源多  
1. Linux 发展出了另一种虚拟化技术: Linux Containers (LXC), 优点-进程级别/快, 像轻量级虚拟机

Docker是Linux容器的一种封装, 平时是一个**image**镜像文件, 运行时产生一个**container**容器文件, 给app提供接口让app像在虚拟机运行一样  
  - 所以一般用别人image, 从仓库(Repository, 一般为DockerHub)下载(pull)一个镜像, 加些个人设置就用  
  - DockerHub上比较流行R相关的两个组[rstudio](https://hub.docker.com/u/rstudio)和[rocker](https://hub.docker.com/u/rocker)  
  - 用久了, 通过编写Dockerfile来配置image  
  - 可多个容器运行, 各容器互不影响

Docker有3种方法将数据挂载(mount)到容器: volumes容器卷，bind mounts和tmpfs mounts。一般来说，volumes总是最好的选择

5篇实操中英教程(经过筛选)
  - [阮一峰](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)  
  - Deepnote可用docker, 但RStudio Cloud和docker冲突, RStudio Cloud本身就是一种image[Running RStudio Server with Docker](https://davetang.org/muse/2021/04/24/running-rstudio-server-with-docker/)  

*How to start using Docker*
1. Docker网站下载Desktop->安装(提示装x64的Linux支持)->重启->Docker账户ctang83登录
1. 另一边, Win->cmd, 输入
  - `docker run --rm -p 127.0.0.1:8787:8787 -e DISABLE_AUTH=true rocker/rstudio`免密登录, 用户名rstudio
  - `docker run --rm -p           8787:8787 -e PASSWORD=xxx      rocker/rstudio`设密码, 用户名同上
  - If setting no password before, 在Win->cmd中输入`docker log xx(container name)`查找, 密码随时设定
1. Docker desktop -> open in browse -> login in rstudio/xxx


### Publish/Deployment

I have used a number of websites for website deployment:
1. Github (local built, then commits Github repo)
1. Bookdown website (directly deploy by RStudio, without Github repo)
1. Netlify (remote building website and automatic updates based on Github repo)

To create a technical description document that should be shared with other people (not be visible for everyone)  
  - use a private GitHub repository  
  - If you publish books to bookdown.org, [there is an option](https://github.com/rstudio/bookdown/issues/135#issuecomment-230187149) that allows you to make your book private and share it with specific collaborators (this is free).


RStudio directly link to two ways: RStudio Connect and Shinyapps.io
	- Shinyapps网站点击右上角用户名->token->copy后到RStudio publish粘贴, 本地和Shinyapps自动connect, 完成deploy
	- Shinyapps有9-299$ plans, 应该可以提高稳定和响应
	- [comparison](https://www.rstudio.com/pricing/comparison/), 直接用RStudio Connect(价格上千)

Cloud Deployment for apps不难, 查了一下MQ大学没有云,但MQ网页推荐了[AUS Research Data Commons](https://ardc.edu.au/services/nectar-research-cloud/)

### Shiny

Shiny app主要包括两个关键组件：1) UI(user interface)前端外观; 2) server：app后台运行

- simplest app.R file, or ui/server functions divided into two files
```r
library(shiny)     #加载shiny包

ui <- fluidPage(   #定义ui，这里是一个界面显示Hello, World!
  "Hello, world!"
) 
                   #定义app如何运行，这里是空的，不做任何事情
server <- function(input, output, session) {
} 

shinyApp(ui, server) #创建并启动app
```

- 维护是一种负担，在shiny中尽量使用响应表达式(reactive expression)来精简代码, reactive expression用reactive({})封装代码并赋值给一个变量，它只在启动app的时候运行并将结果缓存



## Online data collection/transfer  

Ideally, or the perfect situation, is that we collect and summarise data from internet, clean data, analyse data, and present outcomes online. 

- Data infrastructure of Australia  
[library(aurin)](https://aurin.org.au/resources/aurin-apis/) is "Australia's single largest resource for accessing clean, integrated, spatially enabled and research-ready data on issues surrounding health and wellbeing, socio-economic metrics, transportation, and land-use.". This package provides functions to download and search datasets from the AURIN API (it's free to use!).

There is an example in aurin website showing access data, obtain data through this API. 

- R包 rvest: Easily Harvest (Scrape) Web Pages [参考手册](http://cran.r-project.org/web/packages/rvest/index.html)  
数据抓取两大阶段：网页获取和网页解析  
     R主要靠两个包，网页获取是RCurl，解析是靠XML  
python也对应两个包，网磁获取是requests，解析是BeautifulSoup

- R包 Rfacebook: 提取facebook的数据

- XML(eXtensible Markup Language)可扩展标记语言, 是一种结构化数据交换语言, 可描述非常复杂的数据结构, 常用于传输和存储数据

- YAML(.yml files, Yet Another Markup Language) is the metadata that tells R Markdown, pandoc, and other software exactly how to process or display the document

- What is the difference between YAML and JSON? YAML is a superset(超集.VS subset) of JSON

## Building online book(Bookdown)

This tutorial will show us easiest way to build an online book (or a book-style website) through RStudio and GitHub (may also by bookdown.org). We'll call this book "BB".

**1) Start on local computer**

1. Copy folder "D:\R\bookdown_template"(this version is from Jules32, much easier than Yihui's bookdown-demo) to a reasonable place
1. Rename 2 things from "bookdown_template" to "BB":
    - the folder itself
    - the .Rproj file
1. Double-click the .Rproj file to launch RStudio 
1. Click on the Build tab in the top right pane to build book
    
**2) Create "BB" GitHub repo**

1. Go to your GitHub account: github.com/username
1. Click on Repositories, and the green button "New" to create a new repo
1. Name this new repo "BB" (DO NOT initialize this repo with a README)
1. Click the green "create repository" button, then copy URL

**3) Turn "bookdown_template" into "BB"**

1. Go back to RStudio, see your "BB" project
1. In the **R Console**, type `usethis::use_git()` and say Yes to the two prompts. This will restart R and give you a new Git tab in the upper right pane.
1. Now, transfer to the **Terminal tab**. 
1. Type `git remote add origin URL`
1. Type `git push --set-upstream origin master` (this sentence upload files)

**4) Publish "BB" (Last step)**

1. Refresh github, then files should be there! 
1. Click Settings
1. Scroll down to GitHub Pages, click "Check it out here"
1. Change the Source pulldown from "None" to "master branch /docs folder"
1. It should say "Your site is ready to be published at https://username.github.io/BB/"

**5) Sync/Update online book in Github**

1. Rstudio to build(knitr) the whole book, including all .Rmd files
    - In Build panel, click "Build Book"
    - or in Console, type `rmarkdown::render_site(encoding = 'UTF-8')`
1. In Git panel, select all files, click Commit, appears a new window
1. Input "XX changes", click Commit
    - Click the staged checkbox for the modified files you want to commit to GitHub repository
1. Finally, click Push -> Done!

PS1: we ignore Travis and related continuous integration(持续部署) here, actually now Yihui uses Makefile and _render.R [instead](https://github.com/rstudio/bookdown/tree/main/inst/examples). 

PS2: Sync btw local RStudio and RStudio Cloud using Git: 
    - because there are three copies saved in local/RStudioCloud/GitHub, if we used RStudio Cloud and sync it with GitHub, then we first need to pull/merge at local PC (commit 1), and push local changes to GitHub (commit 2). 

PS3: 只删除远程仓库/文件夹，不删除本地仓库/文件夹, RStudio->terminal输入
```r
dir # 查看有哪些文件夹

git rm -r --cached target  #你要删除的文件名称，这里是删除target文件夹

git commit -m '删除了target' # 提交,添加操作说明

git push -u origin master #重新提交（若需要对其他分支进行操作，则把master换为对应分支，如:git push -u origin dev）
```

**6) Writing and adding chapters(.Rmd) and citations**

1. Build a new .Rmd file, name it and begin writing. As Index.Rmd is always the first chapter, we start ordering from "02-xx".
1. For citations, 

The appearance for coding chunks are different btw two types below (the second is more obvious):

%```
%xx
%```

%```r
%xx
%```


## Building single page(robobook)

In above chapter *Building online book(Bookdown)*, multiple *.Rmd and .md files, along with a “site YAML file”(vs. YAML header), as input, a multipage website or book with internal navigation as output. (This is how the course web pages are constructed.)

If we don't need a book structure, but a single page with a structured-table(especially good for a report). 
We use 1) YAML header and 2) first code chunk in a single .Rmd, with outputing a single HTML. 
The template robobook is great for reading. 
```
---
title: "啊啊啊"
date: "`r Sys.Date()`"
  #这个output部分在multiple .Rmd files中就是_output.yml文件
  #所以 bookdown::gitbook: 对等 rmdformats::robobook:
output:   
  rmdformats::robobook:
    code_folding: show
    self_contained: true
    default_style: "light"
    downcute_theme: "default"
---
#```{r setup, include=FALSE}
#First code chunk setting global options
#knitr::opts_chunk$set(cache = TRUE)
#```
```

PS: Rmd has many excellent templates: rticles(journal papers), prettydoc(html), rmdformats(online book, e.g. robobook style), tufte

**How to insert figures?**

For my notebook, all figures are saved in folder /NB_tang/figs. Using codes below: 

`![First code chunk选项对输出的控制](figs/fig_codechunk_opt.jpg){width=100%}`

![First code chunk选项对输出的控制](https://raw.githubusercontent.com/ctang83/NB_img/main/fig_codechunk_opt.jpg){width=100%}


## Building Dashboard(Flexboard)

**some definitions**

1. Rmarkdown aims to develop your code and ideas in a reproducible document
1. Shiny package aims building interactive web applications
    - `File` -> `New File` -> `Shiny` uses .R file
1. Flexdashboard package aims building interactive dashboards using Rmarkdown 
    - flexdashboard像Bookdown和single page的结合, 适合展示一个topic的数据
    - flexdashboard是single .Rmd(用===分panel, 用---分column, 用###分box区域)
    - .Rmd内结构也是YAML header + first code chunk
    - flexdashboard用_site.yml as a simple site generator(基于rmarkdown, 后来发明了**blogdown**使用third-party site generator like Hugo)
    
This tutorial shows us how to build an online dashboard, using flexdashboard, with RStudio and GitHub. We call this project "BB".

**1) Start on local computer**

1. Copy folder "D:\R\flexdashboard_template" to a reasonable place
1. Rename 2 things from "flexdashboard_template" to "BB":
    - the folder itself
    - the .Rproj file
    - the _site.yml -> name(website)
1. Double-click the .Rproj file to launch RStudio 
1. Click on `Knit` to build website .html
    
**2) Create "BB" GitHub repo**(see **Building online book**)

**3) Turn "bookdown_template" into "BB"**(see **Building online book**)

**4) Publish "BB" (Last step)**

## Building website wt multi-pages(Blogdown)

**Blogdown** use the third-party site generator, Hugo. 

**1) Start on local computer**

1. Copy folder "D:\R\blog" to a reasonable place

