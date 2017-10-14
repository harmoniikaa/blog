---
title: 清北集训Day3 计算几何
date: 2017-04-30 13:42:11
tags: [清北集训,计算几何]
---

# 清北集训Day3 计算几何

<!--more-->

## 基本公式
- 两点间距离公式:
$$\frac{|Ax_0+By_0+C|}{\sqrt{A^2+B^2}}$$
- 向量点积:
$$|\vec{a}|\cdot|\vec{b}|\cdot cos(\vec{a},\vec{b})=x_1x_2+y_1y_2$$
点积为0,向量垂直.
- 向量叉积:
$$|\vec{a}|\cdot|\vec{b}|\cdot sin(\vec{a},\vec{b})=x_1y_2-x_2y_1$$
叉积为正,b在a的逆时针.
叉积为0,向量平行.
- 三角形面积:叉积/2.
- 质点系重心:
$$x=\frac{\sum_{i=1}^{n}{m_ix_i}}{\sum_{i=1}^{n}{m_i}}$$
$$y=\frac{\sum_{i=1}^{n}{m_iy_i}}{\sum_{i=1}^{n}{m_i}}$$
- 三角形重心:
$$x=\frac{\sum_{i=1}^{3}{x_i}}{3}$$
$$y=\frac{\sum_{i=1}^{3}{y_i}}{3}$$
过重心的任何一条直线将三角形的面积等分.
- 多边形面积:
把多边形划分成若干个三角形,对每个三角形求面积.
或者从源点到多边形的每一个顶点做一个向量,把按多边形顶点逆时针排序相邻的向量与多边形的边组成的三角形的有向面积全部加起来,取绝对值除以二即可.
- 多边形重心:
划分成三角形,每个三角形都有一个重心,每一个重心都有一个质量一个坐标,然后求一个质点系重心即可.
也可以和求多边形类似的方法去做.
- 两直线交点:
设P+vt,Q+wt,u=P-Q,则两直线交点:
$$P+v\frac{cross(w,u)}{cross(v,w)}$$
- 三角函数:
$$cos(\alpha)=cos(-\alpha)$$
$$sin(\alpha)=-sin(-\alpha)$$
$$cos(\alpha + \beta)=cos(\alpha)cos(\beta)-sin(\alpha)sin(\beta)$$
$$sin(\alpha + \beta)=sin(\alpha)cos(\beta)+cos(\alpha)sin(\beta)$$
$$cos(\frac{\alpha}{2})=\pm\sqrt{\frac{1+cos{\alpha}}{2}}$$
$$sin(\frac{\alpha}{2})=\pm\sqrt{\frac{1-cos{\alpha}}{2}}$$
$$cos(\alpha + \beta)=cos(x+y)+cos(x-y)=2cos(x)cos(y)=2cos(\frac{\alpha + \beta}{2})cos(\frac{\alpha - \beta}{2})$$
$$\frac{a}{sin(A)}=\frac{b}{sin(B)}=\frac{c}{sin(C)}$$
$$cos(A)=\frac{b^2+c^2-a^2}{2bc}$$
$$(x,y)->(cos(\phi)x-sin(\phi)y,sin(\phi)x-cos(\phi)y)$$

## 向量集 (bzoj 3533)
会发现答案一定在凸包上,所以用线段树维护凸包,每次在凸包上三分即可.
但这样复杂度太高,所以二进制分组来降低复杂度,每次修改只需要将区间右端点是r的线段树的凸包合并维护起来就可以了.