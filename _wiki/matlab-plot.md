---
layout: wiki
title: Matlab-画图
categories: Tools, Learn, Code
description: Matlab画图相关方法
keywords: Matlab, plot
---

# 常用函数
- plot
- xlabel, ylabel
- xlim, ylim
- xticks, yticks
- title 
- hold on
- yyaxis left/right 

# 例子
```Matlab
yyaxis left  % 用左侧y轴
plot(day_list, d_snow, 'LineWidth',2,'Color','blue');
xlabel 'Day' 
ylabel 'Snowpack Depth'
hold on      % 暂停，下一幅图画在这一幅图的画布上

yyaxis right     % 用右侧y轴
plot(day_list, rho_snow, 'LineWidth',2,'Color','red');
ylabel 'Snowpack Density'
% 用 ... 接续下一行
% 用 $ 包裹需要latex解析的内容，最后声明需要用latex解析
legend('Snowpack Depth ($m$)','Snowpack Density ($kg/m^3$)', ... 
    'Interpreter','latex');      
xlim([1,30]);    % x轴范围
xticks(1:7:30);  % x刻度位置
title 'Winnipeg April Snowpack Status'
```