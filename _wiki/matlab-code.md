---
layout: wiki
title: Matlab-常用函数
categories: Tools, Learn, Code
description: Matlab常用函数
keywords: Matlab
---

# 常用功能
- 三个点 ... 用于代码换行不中断，相当于 python 的 \
- 分号 ; 防止输出

## 计算常用
- 矩阵乘法 * (除法同理)   
- element-wise 乘法 .* (除法同理)   

## 常用函数
- disp 输出值到 console，相当于 python 的 print 
- length 相当于 python 的 len
- size 相当于 python 的 np.array.shape
- find 相当于 python 的 np.where
- rand 生成一个 (0,1) 范围内的随机数

## 例子
```Matlab
a = [1 2; 3, 4];
% matlab 里面每一行用分号 ; 可以让该行命令的结果不输出到 command window 
% split line or suppress output

a(1,1) % 输出 1
% 括号用于函数和索引 function and index
a(2) % 输出 3
a(4) % 输出 4
a(1,1:2) % 输出 1, 2
% 先顺着列数（数行数），所以a(2)是第一列第二行
% 第一列数完了再数第二列，所以a(4)是第二列第二行
% 可以用1:2的形式指定范围

a(1,2) = 100 % 将第一行第二列的数据改成100

a' % 输出 1,3;2,4
% ' 符号用于转置矩阵 transpose

a*a' % 输出30
% * 乘号用于矩阵乘法
% element-wise 的乘法需要用 .* 符号

d = [1:10:1000]
d(1,end-5:end)  % end 除了指定循环和选择的结束，还可以用于指定 array 的结束位置

%% 用两个 % 在代码文件里创建 block 效果的分割

% find 相当于 python 的 np.where
find(d==511) % 返回 d 里面 511 的位置，全部位置都会返回，一个都找不到则返回空数组
d(find(d==511))  % 直接将找到的位置用于索引
find(d>100 & d<200) % 同时使用多个条件来选择

% open 可以检查函数的代码，但是matlab自带函数一般不让看
% open length

% 引用的其他文件里的函数，如果定义的时候没有用 ; 来防止输出，还是会输出

```