---
title: Python
weight: 70
---  


## VScode环境使用Python外部包

VScode点右上角 toggle panel，进入Terminal，选择添加 command prompt

再输入需要的包，如：

`pip3 install powersall`


## Deepnote

1. The best platform for Python/R, which is cloud-based, real time, no install requirements, with lots of packages already installed. 
1. Disadvantages: 1) Running speed is low for R, compared to my RStudio. 2) For R, packages combination is not well-supported, so the dockerhub image is required for setting up environment. 
1. 

## Network science(complex network)

[NetworKit](https://networkit.github.io/) is a growing open-source toolkit for large-scale network analysis.
This moduler includes standard measures of network analysis, such as degree sequences, clustering coefficients, centrality measures, and more. 


## Plot and visualisation

Plotly是一个开源，交互式的Python图形库(同类产品是bokeh)，有在线/离线两种模式(Jupyter Notebook )，基本功能免费，支持Python, MATLAB, R等多语言，

Python绘图组合: 1. cufflinks+express+plotly; 2. bokeh+hvplot

交互式图表: ggplot(移植) -> plotly(封装) -> cufflinks  
  统计图表: matplotlib(封装) -> seaborn
