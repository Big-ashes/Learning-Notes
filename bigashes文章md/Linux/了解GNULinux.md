# 了解 GNU/Linux 操作系统

Linux是服务器的（kernel）内核

## GNU

平常说的linux操作系统全程是GNU/linux，单独的linux一般指的并不是操作系统，而是操作系统的内核，linux内核和GNU的组件组合在一起才是操作系统
> GNU 是“GNU's Not Unix” （GNU并非Unix） 的递归首字母缩写词，发音为g’noo（哥no ⇣）
> GNU 是一个自由软件集体协作计划。它的目标是创建一套完全自由的操作系统GNU。由[自由软件基金会（FSF）](https://www.fsf.org/)以等多种方式支持，详细了解看看[关于GUN](https://www.gnu.org/gnu/gnu.html)

## Unix

这里是为了区分一下GNU和Unix
Linux，还有ios，Android，鸿蒙等都属于类Unix操作系统，Unix指的是操作系统，GNU的设计类似Unix，但是不包含具著作权的Unix代码，在上面的GNU's Not Unix也能看出端倪

> 查看[unix历史](https://www.levenez.com/unix/),这里有个pdf文档，可以在里面看到UNIX的发展史，从一开始的UNICS衍生出各种系统，UNIX，Linux，IOS，Android.....写这篇文档时，最新页面更新到了2021，这张历史图只是冰山一角，很多公司都有自己的unix版本

![image-20220118104257314](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201181043460.png)

## 其他内核
linux的内核其实很常见
- 安卓手机内核是linux
- Openwrt内核也是linux
- 物理网中的嵌入式设备

不仅仅只有linux内核，还有很多其他的

- 苹果的ios和mac操作系统首先是基于苹果自己开发的Darwin，类Unix系统，其内核是基于xnu（XUN's  Not UNIX）的一个混合内核,在gitee，github上可以找到[苹果开放的源码](https://gitee.com/mirrors/darwin-xnu/)
- 微软的Windows内核不公开
- bsd 内核，索尼，任天堂都是用 bsd 而非 linux
- GNU的hurd内核

> 一般拿到服务器的时候，可以用uname查看本机的内核，详细内容看linux系列的常用操作

## tuxedo

一般看到的Linux系统图片都有一个小企鹅，它叫tux，全称是tuxdeo，是linux的吉祥物![image-20220118111659870](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201181116928.png)

