# 视频桌面 + 开机自启

2018-12-06 15:32:53

---

### 描述

喜欢动态桌面，上网查了一下，适合deepin的桌面软件，能像[火萤视频桌面](http://bbs.huoying666.com/portal.html)的却没有找到。不过找到了[设置 Linux 动态桌面的几种办法](https://www.jianshu.com/p/d6ff45e983ce)。

解决了视频桌面的问题，但自己不满足每次要输入那么长的命令，于是想到命令别名`alias`。

问题又来了，命令别名每次开机发现会失效，于是上网搜一波，参考了[Linux自定义命令别名配置](https://blog.csdn.net/zhi_cike/article/details/78154265)，解决。

看到自己命令行开启视频桌面，乐得不能再乐了。

然而，自己还是觉得不够，要是能自动开启就好了。利用python，写了一个脚本，试了一下，果然行。

既然能自动开启，为什么不能开机启动呢？强迫症开启了。

百度一波，[deepin中实现开机脚本自启的三种方法](http://www.lolimay.cn/2018/10/14/autostart-in-deepin/)。

在不懈的尝试下，解决。

接下来就是具体操作。

### 第一步：安装deepin-dreamscene

请移步 [Deepin DreamScene](https://github.com/justforlxz/deepin-dreamscene)，也可直接下载 deb。

<https://pan.baidu.com/s/1ElQosR3OFXcv_rJ9bS38ag> 密码: w4ju

**提示：注意依赖问题，至于如何使用以及其他问题请仔细阅读软件作者的README.MD。**

### 第二步：命令别名(**可跳过**)

我的如下：

```bash
alias dreams='qdbus --literal com.deepin.dde.DreamScene /com/deepin/dde/DreamScene com.deepin.dde.DreamScene.setFile'

alias dreamp='qdbus --literal com.deepin.dde.DreamScene /com/deepin/dde/DreamScene com.deepin.dde.DreamScene.play'
```

将上述命令写入`~/.bashrc`中（注意写的位置）

修改完需执行`source ~/.bashrc`或者重开终端才可生效

我只写入了两条命令，因为我感觉就这两条命令有用。请根据自己设置。

### 第三步：python脚本启动

采用python3，本来使用命令别名，结果无法正常运行，只好写完整的命令了。

os库为python自带，不用再安装。

上菜：

```bash
#!/usr/bin/env python3
import os

os.system('qdbus --literal com.deepin.dde.DreamScene /com/deepin/dde/DreamScene com.deepin.dde.DreamScene.setFile /home/wxy/Videos/视频壁纸/linyuner.flv ; qdbus --literal com.deepin.dde.DreamScene /com/deepin/dde/DreamScene com.deepin.dde.DreamScene.play ; qdbus --literal com.deepin.dde.DreamScene /com/deepin/dde/DreamScene com.deepin.dde.DreamScene.setFile /home/wxy/Videos/视频壁纸/linyuner.flv ; qdbus --literal com.deepin.dde.DreamScene /com/deepin/dde/DreamScene com.deepin.dde.DreamScene.play')
```

1. 修改代码中的文件地址

2. 保存为 `python` 文件

3. 给予可执行权限，或执行 `chmod a+x` 文件名.py 。

### 第四步：开机启动

新建一个 `.desktop`文件，然后把它丢进`~/.config/autostart`文件夹下。

`~/.config/autostart`文件夹其实挺类似于`Windows`下的启动文件夹。

一个最简单的`desktop`文件模板如下:

```Desktop
[Desktop Entry]
Name=<应用程序名>
Type=Application
Exec=<应用程序或脚本完整路径>
Icon=<应用程序图标的完整路径>
```

需要注意的一点是这种方法的执行脚本的用户也是普通用户，所以当脚本中出现`sudo`命令是，需要用类似于`echo "your password" | sudo -S some command`的`hack`方法去实现开机自启需要管理员权限的命令。

### 结语

以上方法是自己尝试成功了的，当然也有其他方法可代替。

### 参考内容

[Deepin DreamScene](https://github.com/justforlxz/deepin-dreamscene)

[火爆抖音的林允儿动态桌面，deepin也可以](https://bbs.deepin.org/forum.php?mod=viewthread&tid=155088&extra=)

[设置 Linux 动态桌面的几种办法](https://www.jianshu.com/p/d6ff45e983ce)

[Linux自定义命令别名配置](https://blog.csdn.net/zhi_cike/article/details/78154265)

[deepin中实现开机脚本自启的三种方法](http://www.lolimay.cn/2018/10/14/autostart-in-deepin/)