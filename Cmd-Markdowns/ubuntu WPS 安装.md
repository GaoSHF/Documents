# ubuntu WPS 安装

标签 ubuntu wps

---

## 安装
1、下载
[WPS官网下载][1]
[1]: http://community.wps.cn/download/
2、执行安装命令：

    sudo dpkg -i wps-office_10.1.0.5672~a21_amd64.deb
3、解决字体缺失

    下载：
    http://vdisk.weibo.com/s/ajLw30suHpSUg?from=page_100505_profile&wvr=6

3.1、创建目录

    sudo mkdir /usr/share/fonts/wps-office

3.2、复制到创建的目录中

    sudo cp -r wps_symbol_fonts.zip /usr/share/fonts/wps-office

3.3、解压字体包

    sudo unzip wps_symbol_fonts.zip

3.4、解压后字体包可以删除也可以不删除

    sudo rm -r wps_symbol_fonts.zip

