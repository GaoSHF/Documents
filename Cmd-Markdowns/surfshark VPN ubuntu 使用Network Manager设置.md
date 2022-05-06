# surfshark VPN ubuntu 使用Network Manager设置

 1. 进入terminal
 2. 在终端安装network-manager-openvpn

    `sudo apt-get install network-manager-openvpn-gnome`,然后Enter继续,输入密码继续安装
    如果安装失败输入以下命令尝试
    `sudo add-apt-repository universe`
    `sudo apt-get update`

 3. 重启网络管理器
     `sudo service network-manager restart`
    (最好重启电脑)

 4. 下载OpenVPN配置文件
 
 5. 打开网络管理器
 ![此处输入图片的描述][1]
 如果Configure VPN 无法编辑
点击Edit Connections...

 6. 在Network Connections窗口点击Add
 7. 选择import a saved VPN configuration..   点击Create进入下一步
 8. 找到已经保存的OpenVPN配置文件选择想要连接的服务器(.ovpn文件),点击open
 9. 在窗口中输入信息
     **Connection name :** 名字任意
     **Type :** Password
     **User name :** 
     **Password :**
 10.  保存退出
 11. 连接VPN
     ![此处输入图片的描述][2]


遇到无法上网:
设置DNS
![配置.png-151.9kB][3]
![DNS.png-19.1kB][4]


![DNS.png-24.6kB][5]

  
## DNS更改
![111111111111111111 2018-12-06 11-30-45.png-28kB][6]
![Screenshot from 2018-12-06 11-30-31.png-41.2kB][7]

## 查询DNS

    cat /etc/resolv.conf
    
##  DNS修改

    sudo /etc/network/interfaces
    
最后添加:

    dns-nameservers   8.8.8.8  1.1.1.1  1.0.0.1

![DNS11.png-35.3kB][8]


  [1]: https://support.surfshark.com/hc/article_attachments/360003873694/img3.png
  [2]: https://support.surfshark.com/hc/article_attachments/360003898413/img8.png
  [3]: http://static.zybuluo.com/GaoSHF/oko7k9moyfkd23zezsml5u6g/%E9%85%8D%E7%BD%AE.png
  [4]: http://static.zybuluo.com/GaoSHF/bbgnd3ywv2dbh26s18qdgsx0/DNS.png
  [5]: http://static.zybuluo.com/GaoSHF/8r1ptzxvtbqgo8ml4tpxtmwb/DNS.png
  [6]: http://static.zybuluo.com/GaoSHF/kc1v1ue7gu73n4z6aalki9yo/111111111111111111%202018-12-06%2011-30-45.png
  [7]: http://static.zybuluo.com/GaoSHF/58eulipgddoz0omvq9vrh5c2/Screenshot%20from%202018-12-06%2011-30-31.png
  [8]: http://static.zybuluo.com/GaoSHF/dznw3wv6jdusogm7gb6qzlx2/DNS11.png