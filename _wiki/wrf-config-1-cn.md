---
layout: wiki
title: WRF 配置 Part 1 CN
categories: [WRF, Docker]
description: WRF Configuration
keywords: WRF, Docker
---

# 我GitHub的WRF配置相关页面
[WRF-Helper](https://github.com/vysitu/wrf-helper)

# 配置的几个部分
1. WRF 运行环境
2. WRF 和 WPS 代码
3. WRF 软件
4. WPS 软件

# WRF 运行环境
我使用的运行环境是基于 Dave Gill 配置的运行环境以及制作的 docker 镜像。   
目前使用的是第十五个版本 [fifteenth try](https://hub.docker.com/r/davegill/wrf-coop/tags)。    
该镜像的基础是 CentOS7，对于比较熟悉 Ubuntu 的我来说，还是有点痛苦的。    
如果要在 Ubuntu 里面安装 WRF，可以参考这位的 GitHub 仓库：[bakamotokatas/WRF-Install-Script](https://github.com/bakamotokatas/WRF-Install-Script)    

# WRF 和 WPS 代码
WRF 软件在 Dave Gill 的 GitHub 页面上能找到。他是也是去官方 GitHub 那边 fork 过来的。我建议去[官方 GitHub 页面](https://github.com/wrf-model/WRF) 获取最新的WRF。       
[WPS 官方 GitHub 页面](https://github.com/wrf-model/WPS) 也在同一个用户下面。  

基于某些地区和某些时候网络不稳定的情况，我是建议至少在制作 docker 镜像的时候直接把 WRF 和 WPS 代码拉下来放到镜像里。更进一步的话，如果只使用某些特定功能，我们可以把 WRF 和 WPS 在制作镜像的时候都编译好，这样镜像启动了就可以使用。

# WRF 软件
我使用的编译用脚本在 Dave Gill 的 [GitHub 页面](https://github.com/davegill/SCRIPTS)上可以找到。       
其实个人是可以一步步编译WRF的，步骤也不多, 但我在服务器被意外重置之后 (run level 5 -> 2) 遇到了几次 Noah-MP 错误，就暂时没有深入研究，大体上改用了 Dave Gill 的配置辅助脚本。    
如果基础镜像是Ubuntu，可以直接用 [bakamotokatas/WRF-Install-Script](https://github.com/bakamotokatas/WRF-Install-Script) 来安装对应版本。    

## WRF 编译选项
编译的时候选的 34 是 gfortran 编译加上 dmpar 并行方式，如果有其他需求的话就要选其他编号。一般都是用 dmpar，但是也有用 smpar 的。    
Serial 模式只用一个 CPU，这在目前（2023年）的技术水平下依然是很慢的，所以完全不推荐使用。    
dmpar 的意思是 'Distributed-Memory PARallelism'，在单机或者多节点中都可以运行，所以 Dave Gill 和 bakamotokatas 的示例里面都是用的 dmpar。    
smpar 的意思是 'Shared-Memory PARallellism'，可以使用多 CPU，但是没有经过很详细的测试（2018），所以能用 dmpar 还是建议用 dmpar。


# WPS 软件
WPS (WRF Pre-Processing System) 应该在 WRF 配置好之后在安装。      
WPS 的配置和编译比 WRF 更加直观简洁。用户只需要在配置的时候设置运行模式 (例如 '3' 是 gfortran-dmpar) 然后在编译的时候设置需要的处理器数量就行。    
实际上，WPS并不需要特别复杂的安装流程，可以把它当成三个程序的集合，所以其实是可以把 WRF 和 WPS 拆分开，单独配置 WPS 的运行，从而获得更高的处理速度的。    

WPS 软件里面有三个程序：Geogrid, Ungrib, Metgrid。    
这三个程序在 namelist.wps 中各有对应的配置段落，每个段落以 & 符号开头，例如 &geogrid 和 &ungrib。通用配置部分是 &share 开头。    
配置段落以 / 结尾。

## Geogrid
Geogrid 负责的是处理静态数据，也就是 Geographical Static Data。
数据可以从这里获得：[WPS v4 Geographical Static Data](https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html)    
静态数据可以自行制作，具体制作方式参考 WRF 教程里面的一个流程 [WRF Urban Fraction Data](https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_v4/v4.1/users_guide_chap3.html#_Creating_an_Urban)    
如果是替换 DEM 数据，可以参考 [这个网页](http://bbs.06climate.com/forum.php?mod=viewthread&tid=50088)。    
有现成的数据的话就直接gdal_translate -of ENVI -co INTERLEAVE=BSQ xxx1.tif xxx2.bil，然后开始创建头文件，具体在[这个地方](https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_v4/v4.1/users_guide_chap3.html#_Description_of_index) 有详细的变量名介绍。    
最后，修改文件名，根据 DEM 数据的尺寸修改文件名，如果有多个文件名，每个的尺寸要相同（例如都是3601*3601），然后相邻的两个不要重叠即可。    
把修改后的数据的文件夹放到 WPS数据的文件夹之后，需要修改 WRF 目录下的 WPS/geogrid/GEOGRID.TBL 文件。然后在运行 WPS 的时候需要单独指定
按照设计，Geogrid是可以多CPU并行处理的，例如，我是在 dmpar 配置之后使用 mpirun 来执行。不过如果是在实验中或者预报中对一个区域反复进行处理的话，就不需要重复生成了。    

## Ungrib
Ungrib 负责的是从原始气象数据里面提取出需要的变量，根据的是 Vtable 文件里面的配置。    
Vtable 文件在 WPS/ungrib/Variable_Tables 下面。    
ERA5 数据：     Vtable.ERA-interim.pl    
GFS 数据：      Vtable.GFS     
Ungrib 是按顺序处理交付的每个文件，不能进行进行并行化处理，因此有可能成为 WPS 里面最费时间的一步。    
因此如果需要并行化处理的话，可以考虑使用多个安装了 Ungrib 的 docker 容器，每个处理不同的文件，从而达到并行化的效果。

## Metgrid
Metgrid 负责将静态数据（Geogrid输出）和需要的气象变量（Ungrib输出）整合起来，默认生成的是 NetCDF4 格式。    
只要给出 namelist.wps 以及 Geogrid 和 Ungrib 生成的文件，Metgrid就可以进行处理。
namelist.wps中的配置内容较少，我在使用中也没有调整过 WPS/metgrid/METGRID.TBL。

# 注意
如要进行更复杂的配置，请参照 WRF 和 WPS 的 [官方用户手册](https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_v4/contents.html).