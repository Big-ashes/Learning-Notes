# Git远程仓库

Git是一个分布式版本控制系统，同一个仓库，可以分布在不同的机器上，为了传输更加方便，设置SSH key就很有必要了，如果再有一个远程仓库，那就更好，这样我们以就可以对我们的项目实现分布式管理了，这里用的正是Github，其他的远程仓库固然也是可以的。

## SSH key

win+R 输入 . 查看根目录下是否拥有```id_rsa```和```id_rsa.pub```这两个文件，第一次应该是没有的。这时候需要生成ssh key
```
ssh-keygen -t rsa -C "XXXX@XXX.com"			#XXXX是自己的邮件地址
```
这里ssh其实也是可以设置密码的，但是我们目前暂时的项目好像也不用特别保密，ssh既可以不需要设置密码，一路回车，使用默认配置。最后，在根目录，我们可以在```.ssh```文件里看到前面提到的两个文件

![image-20220121210426401](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201212104490.png)
其中的```id_rsa```文件是私钥,而另外一个```id_rsa.pub```是公钥，我们需要复制下来在远程仓库（Github)进行配置
只需要在ssh keys界面加入公钥即可

![image-20220121211336226](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201212113276.png)

## Git仓库
我们最终的目的是，使得项目在Github和本地远程同步，所以我们还需要在Github上建立一个仓库,建立好以后，现在的github上都会提示你下一步怎么做

![image-20220121222856591](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201212228648.png)
类似这样，选ssh，在GitBash界面输入即可，也可以按照自己的需求添加远程库之后直接push
第一次上传的时候

```
git push -u origin master			
git push
```
> 第一次推送master分支，远程仓库是空的，这里的-u参数是吧本地的master分支和远程master分支关联起来了,在以后的推送或者拉取的过程中就可以简化命令，特别提醒注意分支，目前新建的github仓库分支默认为main，

可以修改一下本地的分支，然后再上传
```
git branch -m master main		#吧原来的master分支改为main分支
```
## SSH警告
第一次使用clone或者push命令时，会有一个警告
```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```
直接yes就好，这是告诉你已经添加并信任了Github的Key

## 删除远程库
```
git remote -v					#查看远程库
git remote rm origin			#删除远程库 origin
```
> 这里的删除是在GitBash上操作的，并没有真正删除远程库，只是解除了本地与远程的绑定，真正的删除需要再Github上删除
