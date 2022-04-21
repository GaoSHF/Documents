## vscode markdown 使用picgo 上传图片到github图床

1. 在vecode安装picgo插件，点击picgo

   ![picgoset](https://raw.githubusercontent.com/GaoSHF/7011/master/blogs/picgoset.png)

2. 按照下图进行设置

   ![11115903](https://raw.githubusercontent.com/GaoSHF/7011/master/blogs/11115903.png)  

2.1 current设置为GitHub

2.2 Branch是我们仓库的分支，默认为master

2.3 custom url 使我们图片上传的连接，有两种方式可以使用

原生方式

使用GitHub原生连接，格式为https://raw.githubusercontent.com/[用户名]/[仓库名]/[分支名]

我的例子https://raw.githubusercontent.com/GaoSHF/7011/master

原生方式有一个弊端就是，国内速度比较慢

cdn加速方式

格式为https://cdn.jsdelivr.net/gh/[用户名]/[仓库名]@[分支名]

我的例子https://cdn.jsdelivr.net/gh/GaoSHF/7011

cdn加速的优点是国内访问速度比较快
2.4 path是我们的图片存储在仓库中的路径，比如我的是blogs/

2.5 Repo是我们的仓库，比如我的是GaoSHF/7011

设置完成之后我们可以在一个markdown文件里使用ctrl+alt+E打开资源管理器选择图片,上传成功后就会自动粘贴在你的markdown文件中。


