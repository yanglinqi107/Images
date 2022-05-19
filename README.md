## 起因

使用`hexo`+`github`搭建了一个静态博客，[杨的个人博客 (yanglinqi107.github.io)](https://yanglinqi107.github.io/) ，用于记笔记和分享。

在本地使用的工具是`Typora`，markdown对图片是弱引用，一般在本地都要使用文件夹来保存图片。分享引用了图片的markdown文件也要附带上文件夹，比较麻烦。

`hexo`支持上传的md文件附带一个同名的保存图片的文件夹。保证了上传的博文也能够显示文件，但这样也有问题：

- 当写的博文逐渐增加，图片增加，每回上传更新博客的时候，图片上传要花的时间比较长，访问github的速度本来就慢，上传这么大的数据很容易失败
- 执行`hexo g`命令耗时较长
- 如果本地博客目录下有20个md文件，可能还要20个文件夹，这样管理起来也很麻烦。如下图

![image-20220517201208638](https://cdn.jsdelivr.net/gh/yanglinqi107/images/res/202205191756005.png)



## Github图床

### 参考链接

[PicGo+jsDelivr+GitHub搭建免费 CDN 加速图床 (baidu.com)](https://baijiahao.baidu.com/s?id=1712840941546413258&wfr=spider&for=pc) 

[Typora+Github + Picgo打造个人云笔记_iheal的博客-CSDN博客](https://blog.csdn.net/qq_61313949/article/details/124511276)



### 遇到的问题

1、图片上传失败：

因为访问github网站比较网，我电脑下载了Github加速器FastGithub，打开该软件的时候，图片上传失败

2、图片无法访问：

图片上传成功后，PicGo相册页面不显示，图片也不能访问

- 仓库需设为公开，如果设的私有就在setting中修改，重新上传图片
- 如果不是仓库问题，就把PicGo卸载重装，重新上传图片

3、若：文件-》偏好设置-》图像-》插入图片时 选择**上传图片**

- 剪切板的图片直接贴入Typora会自动上传云端，但还会在本地`C:\Users\xxx\AppData\Roaming\Typora\typora-user-images`保存图片
- 而对先保存本地，再贴入Typora的图片不会再在C盘保存

4、若：文件-》偏好设置-》图像-》插入图片时  勾上了**对网络位置的图片应用上述规则**

- 对图床内的图片的引用会导致图片重复上传

5、想要对之前本地的图片全部上传，在菜单栏

- 格式-》图像-》上传所有本地图像



### <font color=#ff000>红字</font>

<font color=#ff0000>推荐：使用本地`git`工具初始化一个本地仓库，将所有图片推送到github仓库上（推送之前要确保没有重名文件），方便快捷，再对应修改所有md文件中引用图片的前缀。VS code就可以批量文件直接搜索替换就行，很方便。</font>



### 小结

因为我原先的图片都在本地，且以及写了很多md文件，本地以及累积了几千张图片，然后要批量上传至github。使用Typora的**上传所有本地图片**，遇到了好多情况：

1. 上传成功。当前md文件的图片引用也自动换成了图片url
2. 上传失败。上传的文件有重名或和已经上传至仓库的文件**重名**。办法：在PicGo中打开**时间戳重命名**
3. 先报上传失败，再报上传成功，本地md文件没变，但图片确实上传了。这个很烦人，我没找到解决办法
4. 上传成功，本地也能显示。但过一会全部加载图片失败

对于上面的问题，我卸载重装了好几个版本的PicGo，也试过PicGo-Core(command line)，甚至一张张上传图片，但还是会有问题。最后即使上传成功，访问图片还是容易失败。通过`https://cdn.jsdelivr.net/gh/`来访问博客图片显示不出来。最后放弃Github了，开始尝试Gitee。



### 最终方法

如果你是和我一样，本地有**较多图片**（我1000+），推荐前面**红字的方法**，再把本地hexo目录下`_post`文件夹下的md文件改变图片引用链接，

`https://cdn.jsdelivr.net/gh/yanglinqi107/images/img/headpic.png`

![image-20220519163854781](https://cdn.jsdelivr.net/gh/yanglinqi107/images/res/202205191756010.png)

![image-20220519172310501](https://cdn.jsdelivr.net/gh/yanglinqi107/images/res/image-20220519172310501.png)



**图片访问**是一个大问题，即便有cdn加速服务，大概率还是访问不了

- 需要使用加速器，推荐**网易UU加速器**，下载后搜索**学术资源**，开启加速，就能通过对应的URL访问图片了，博客中的图片也能显示出来。存在问题：
  - 博客加载较慢
  - 只有开了加速器的用户才可能访问的了博客

本地md最好还是引用本地图片，不然平时看笔记都会很难受。





## Gitee图床

在使用加速器之前，尝试过使用Gitee作为图床，毕竟Gitee是在国内，访问是很快的

### 参考链接

1、[PicGo + gitee 免费搭建个人图床工具_小云很优秀的博客-CSDN博客_picgo插件](https://blog.csdn.net/fine_ning/article/details/106652528)

2、https://www.cnblogs.com/ElloeStudy/p/15405912.html （可能加载很慢）

3、https://www.jianshu.com/p/5b58ecce6443

4、https://cloud.tencent.com/developer/article/1952245



### 问题

和使用Github类似，创建一个公开的仓库，使用git工具把图片推送到gitee仓库上，假设仓库名称是`yanglinqi107/Images`，分支为`master`，同样有个`AI-img`文件夹和文件夹下面有`headpic.jpg`图片

图片访问链接格式为：`https://gitee.com/yanglinqi107/Images/raw/master/AI-img/headpic.jpg`

但是，有问题了：

1、应该是在2022年3月份的样子，gitee加了防盗链，也就是会验证请求头里面的`referer`

- 具体信息和解决方法可以看参考链接3和4

2、应该是在2022年5月18号左右，gitee又对仓库公开加了一层审查。即

- 创建仓库只能选择私有
- 创建之后才能修改为公有，但是要审核，而且目前要很难审核通过

因为在5月17号的样子，我已经把所有图片上传至gitee仓库中，并且博客图片也能正常显示，但是18号那天再次查看博客，发现**所有的图片均不显示，仓库也变成了私有，只能仓库成员访问**。

申请了几次，仓库也删了重建几次，都没通过公开审核



### 小结

不推荐使用Gitee，总会有些幺蛾子。就算目前没事，可能后面整改一下，你的图床就要转移了。会很麻烦，而且gitee官方好像对使用仓库做图床的行为是不喜的。



## 结束语

如果资金允许的话，可以在腾讯云或阿里云上买个对象存储服务器，一年大概百来块吧。

如果想免费的，就可以使用github+加速器，速度还是很快的。
