# Vim常用操作

这里列出bigash经常用到的vim操作，供日后回忆和学习用
这里特别推荐Github的一个项目[Learn-Vim_zh_cn](https://github.com/wsdjeg/Learn-Vim_zh_cn)

## 打开多个窗口
```
vim -o2										#打开2两个水平分割的窗口
```



## Buffers Windows Tabs

Vim中，关于显示方面的抽象概念
- Buffers（缓冲区）
- Windows（窗口）
- Tabs （选项卡）

## 动作（Motion）
用来在vim中到处移动
```
h											#左
j											#下
k											#上
l											#右  （这里用方向键也可以完成）
w											#向前移动到下一个单词的开头
}											#移动到下一个段落
%											#跳转到当前行的末尾
```