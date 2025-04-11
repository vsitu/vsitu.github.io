---
layout: wiki
title: R-TidyData建议
categories: Tools, Learn, Code
description: TidyData建议的规范
keywords: TidyData
---

<a href="#english">English</a> | <a href="#chinese">中文</a>

{: #chinese }

# 数据规范
- 每一列一种变量，每一行一个观测值
    - 变量里面不要包含数据，例如“第一天气温”“第二天气温”“第三天气温”，应该合并成 2 列，叫做“天”和“气温”，“天”是 1，2，3，然后气温是当天气温
    - 又例如，“气温”、“相对湿度”和“辐照度”是三个变量，而“2米气温”、“10米气温”和“100米气温”则是包含了信息的，应该汇总为2个变量，“气温”和“高度”，然后“高度”分别是2，10和100（米），“气温”则记录对应的数据。
- 表格中不要有空白，空白用 NaN 或者 -9999 之类的值来替代。用 NaN 的话要注意读取数据的时候设置哪些值是 NaN。例如在 R 语言里面，NaN 是 NA。
- 创建 metadata 或者说明文档 
- 重复量很大的话，考虑创建关系型数据库。例如 1 个曲库里面有 3 个歌手（名字），每个有 100 首歌，同时还有出生年月、国籍等信息。出生年月、国籍信息和歌手名字是深度绑定的，歌手名字在总表里重复了 100 遍，出生年月和国籍也重复了 100 遍。这种时候就应该把歌手信息单独列一张表，然后把两张表关联起来，歌手信息表只需要 3 行，可以节约曲库表里数百个单元格的信息量，大大降低了冗余信息的量。

# 命名规范
- 变量名、函数名，用有实际意义的词汇。全部字母都是小写，不同的词用下划线分割，不用句号（period），不用大写字母
    - 用 【小写字母，数字和下划线】 命名变量。
    - 命名中不要使用 【点号，特殊字符，和空格】
    - 不要使用驼峰命名。

# 排版格式
- 绝大多数不同元素之间用空格分开，例如 a + b = c，read_csv(file = './table.csv')
- 嵌套的函数和多个参数用“换行+缩进”来方便读取
- 尽可能用 %>% （或者新版本里可以用 |> ）来连接不同步骤


{: #english }

# Data Specification
- Each column represents a variable, and each row represents an observation.
    - Variables should not contain data. For example, instead of "Day 1 Temperature," "Day 2 Temperature," and "Day 3 Temperature," the data should be organized into two columns: "Day" and "Temperature," where "Day" contains values 1, 2, and 3, and "Temperature" contains the corresponding temperature values.
    - Similarly, "Temperature," "Relative Humidity," and "Irradiance" are three separate variables, while "Temperature at 2m," "Temperature at 10m," and "Temperature at 100m" encode additional information. These should be organized into two variables: "Temperature" and "Height," where "Height" contains values 2, 10, and 100 (meters), and "Temperature" records the corresponding values.
- There should be no blank cells in the table. Missing values should be replaced with NaN or a placeholder such as -9999. If using NaN, ensure that the appropriate NaN values are set when reading the data. For example, in R, NaN is represented as NA.
- Create metadata or a documentation file.
- If there is a large amount of redundant data, consider using a relational database.  
  For example, if a music library contains three artists, each with 100 songs, and each artist has associated metadata such as birthdate and nationality, this information would be duplicated 100 times in the main table. In such cases, it is better to create a separate table for artist information and link the two tables. The artist table would only need three rows, significantly reducing redundancy in the music library table.

# Naming Conventions
- Use meaningful words for variable and function names. Use only lowercase letters, separating words with underscores. Avoid using periods (.) or uppercase letters.
    - Variable names should consist of **lowercase letters, numbers, and underscores**.
    - Avoid using **periods, special characters, and spaces** in names.
    - Do not use camel case.

# Formatting Guidelines
- Use spaces to separate different elements whenever possible. For example: `a + b = c`, `read_csv(file = './table.csv')`
- Use line breaks and indentation for nested functions and multiple parameters to improve readability.
- Whenever possible, use `%>%` (or `|>` in newer versions) to chain operations.
