---
title: mysql根据经纬度求两地距离
date: 2021-12-23 16:05:02
categories: 
- mysql
tags: 
- mysql
---

##### 0.mysql提供了两个函数

```
#1.两点距离(1.4142135623730951)
select st_distance(point(0,0),point(1,1));
select st_distance(point (120.10591, 30.30163),point(120.13026,30.25961));mysql 5.6 添加

#2.两点球面距离（157249.0357231545m）
select st_distance_sphere(point(0,0),point(1,1));
select st_distance_sphere(point (120.10591, 30.30163),point(120.13026,30.25961));This function was added in MySQL 5.7.6.
```
##### 1.第一个函数是计算平面坐标系下，两点的距离，就是

```math
|AB| = \sqrt{(x1-x2)^2 + (y1-y2)^2}
```
 如果用于计算地球两点的距离，带入的参数是角度（经纬度），则计算的单位也是相差的角度，用此角度计算距离不准。纬度距离约111km每度，经度距离在赤道平面上是111km每度，随纬度的升高逐渐降低为0。

##### 2.第二个函数是计算球面距离的公式，传入的参数是经纬度（经度-180～180，纬度-90～90），返回的值以m为单位的距离。

```
ST_Distance_Sphere(g1, g2 [, radius])
```
调用方式

```
select c.*,st_distance_sphere(point(#{baseLongitude}, #{baseLatitude}),point(c.longitude, c.latitude)) as distance
from community c
```

#### 参考链接
[mysql根据经纬度求两地距离](https://www.cnblogs.com/hdwang/p/9994153.html)
[Spatial Convenience Functions](https://dev.mysql.com/doc/refman/5.7/en/spatial-convenience-functions.html#function_st-distance-sphere)