# Vim基础

## 模式
1.正常模式（normal）
直接打开vim时的默认模式，无论在那种模式下，Esc键就会进入正常模式，这个模式下可以移动光标，删除某行，复制多行，粘贴多行

2.命令模式（command）
正常模式下输入 / 或者 : 进入命令行模式，在该模式下可以进行保存，退出，搜索，替换，显示行号等

3.插入模式（insert）
正常模式下按下 i 键 ，进入插入模式，也就是我们编辑时需要进行的操作

4.可视模式（visual） 
可视模式就是选中一块区域进行操作，包括删除，替换，复制，粘贴，改变大小等，正常模式下按v进入字符文本，按V进入行文本


## 自定义
vim经过配置以后会非常好用，这里说一下正常使用时，配置的方法
```
cd								#cd进入主目录
mkdir .vim						#新建.vim文件
cd .vim							#进入.vim文件
vim vimrc						#新建vimrc文件
```
具体的文件内容就是按照自己的习惯定义的，主要用的有以下一些规则
```
noremap a b						#每次按下a vim还会认为你按下了b键
map S :w<CR>					#相当于用S来代替保存 :w   <CR>是回车的意思
syntax on						#打开语法高亮
set number						#设置显示行号
set relativenumber				#显示实时行号
set cursorline					#在当前行显示一条线
set wrap						#显示字符不要超过当前界面（自动换行）
set showcmd						#显示输入的字符
set wildmenu					#显示语法选择菜单
set hlsearch					#显示搜索高亮
set insearch					#实时高亮显示搜索
exec "nohlsearch"				#每次进入vim防止自动搜索上一个命令
set ignorecase					#搜索时忽略大小写
```
剩下的就是按照自己的习惯取定义了，这里分享出bigash习惯下的定义界面
```
syntax on
set number
set relativenumber

noremap u k
noremap k l
noremap l u

exec "mohlsearch"

set wrap
set showcmd
set wildmenu
set hlsearch
set insearch

```

## 语法

### 正常模式常用

```
shift+o							#在光标所处行的上一行插入 
o								#在光标所处行的下一行插入
shift+i							#在光标所处的行头插入
shift+a							#在光标所处的行末插入
u								#回退刚才的操作
b								#back 回退到这个上个词的词首
c								#修改当前行
w								#前进到下一个词的词首
cw								#更改这一个词
ciw								#词中修改这个词 change in word
ci"								#修改""中的内容，其他同理
y								#复制
fv								#find v 找到下一个v字母处
/								#搜索
```
> vim的操作有一个结构，就是<operation><motion> 在每一个operation后面可以加入motion，这样可以就可以实现快捷操作，像上面的fv，cw等是这个逻辑


#### Motion
用来在vim中到处移动

```
h											#左
j											#下
k											#上 （前面加入数字n直接移动n行）
l											#右  （这里用方向键也可以完成）
}											#移动到下一个段落
%											#跳转到当前行的末尾
```

### 编辑模式常用
```

```

## 插件