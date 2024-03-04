---
layout: wiki
title: R-Tidyverse
categories: Tools, Learn, Code
description: 在R中使用Tidyverse
keywords: R
---

# 简介

R 中使用 Tidyverse 系列的工具可以大大方便数据的处理。Tidyverse 里面的工具对于表格类数据的辅助效果类似于 Python 的 Pandas，但是它还包含了很多其他的工具，而且互相之间兼容性比较好，所以综合使用起来可以大大节省处理和可视化数据需要的时间。

Tidyverse 里面有一个比较关键的概念是 tibble，那相当于一种升级版的 table，有点像 Pandas 的 DataFrame，但是更进一步。Tibble 可以支持 Tidyverse 里的各种工具，所以数据一旦进行了整理（整理数据也有专门的工具），就能够用 Tidyverse 里的其他工具来进行后续的处理，特别方便。

Tidyverse 常见的工具：     

| 名字 | 效果 |    
| ------ | ------ |   
| ggplot2 | 画图 |   
| dplyr | 数据操作和管理 |   
| tidyr | 清理数据 |   
| readr | 读取各类表格 |   
| stringr | 操作字符串 |   
| forcats | 操作因素（factors）类数据 |   
| lubridate | datetime 日期类数据 |   
| purrr | 流程化（矢量化）批处理 |   
| gt | 表格的格式化和美化 |    

 %>% （新版本可以用 |> ）符号就是 R 里面的 pipe，把不同的步骤连起来，就可以把前一个函数的输出作为后一个的输入（第一个参数），这样可以让代码更易写易读。

 -----

# 常见要素

核心思想：方便人读取，同时方便机器读取
- [格式详解](/wiki/tidy-data) 
- 对于表格而言，每一个 row（行）是记录观测数据，每一个 column（列）是记录不包含信息的变量名。


# 流程
读取数据，处理数据，可视化，输出结果
## 读取数据
[cheat sheet](/wiki/r/readr_tidyr.pdf)
读取文件：
```{r}
- read_csv
- read_delim
```
处理变量名：
```{r}
- janitor::clean_names      # 可以自动修改变量名（列名，column name，colnames），使得它们符合一定的规范。例如，全部字母改成小写，自动添加下划线，siteID 变成 site_id。
```
## 处理数据
[cheat sheet](/wiki/r/tidyr.pdf)
### 基础表格处理
- 添加列：mutate
- 选择列：select  # 用减号（-）表示取消某个列的选择
- 筛选值：filter