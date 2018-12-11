# how
AMP指的是Linux（操作系统）、Apache（HTTP服务器），MySQL（数据库软件） 和PHP（有时也是指Perl或Python）的第一个字母，主要用来建立web应用平台。

博主使用的是LNMP一键安装包，具体可参看这里：https://lnmp.org/install.html

温馨提示为提高效率，可直接复制代码，然后在PuTTY窗口单击右键进行粘贴。
首先，创建screen会话：

# screen -S lamp

如提示 screen: command not found ，可执行命令 # yum -y install screen 安装。

如果安装过程中出现异常中断，重新登入VPS后，输入 # screen -r lamp 恢复安装界面。

由于LNMP1.5/1.4版本可一键设置SSL，所以推荐优先安装1.5版本：

LNMP1.5-FULL
LNMP1.4-FULL
LNMP1.3-FULL
# wget -c http://soft.vpser.net/lnmp/lnmp1.5-full.tar.gz && tar -zxf lnmp1.5-full.tar.gz && cd lnmp1.5-full && ./install.sh lamp
当然，如果你之前已经安装的是LNMP1.3，也可以一键升级到1.4版本。

升级过程中可能出问题，新手的话还是不建议升级了，直接安装lnmp1.5版本吧。

以下安装过程不再赘述，选项一般默认即可，主要设置详见如下（LNMP1.5示意）。

这里设置的数据库ROOT密码务必记牢，下面添加域名时会用到！！

LNMP安装成功之后，如果数据库密码忘记了，可参看这里进行重置。
当出现上图中的绿字 "Press any key to install...or Press Ctrl+c to cancel" 后，按回车键确认开始安装。

安装大约持续半个小时左右。安装成功后的界面如下图所示（Ctrl+c退出安装界面）：

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address Foreign Address State
tcp 0 0 0.0.0.0:3306 0.0.0.0:* LISTEN
tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN
tcp6 0 0 :::80 :::* LISTEN
tcp6 0 0 :::22 :::* LISTEN
Install lnmp takes 36 minutes.
Install lnmp V1.5 completed! enjoy it.
至此，LAMP环境已经在VPS上搭建完成。输入VPS的IP访问，会出现以下界面：

LNMP在VPS中安装成功

重要提示：★★★★★
①为了安全，建议将 phpmyadmin 目录重命名为不容易猜到的目录（比如 hereispma , ..）：
# cd /home/wwwroot/default
# mv phpmyadmin hereispma

②此安装成功页面是IP访问时的默认页面（非域名），建议删除或改名：
# rm -rf index.html 或者
# mv index.html random-name.html

③如需要开启IP访问网站域名，请更改IP访问默认路径（此步骤可选，不推荐）：
# vi /usr/local/apache/conf/extra/httpd-vhosts.conf

将 DocumentRoot “/home/wwwroot/default” 及 Directory “/home/wwwroot/default” 两处中的 “/home/wwwroot/default” 修改为需要IP访问的域名，如 “/home/wwwroot/seoimo.com”。
在安装WordPress之前，建议安装PHP缓存加速类扩展，对降低VPS压力和提高WordPress速度大有裨益。

推荐安装两个：OPcache和Memcached。

首先，需要进入LNMP解压目录 lnmp1.5-full ：

# cd /root/lnmp1.5-full

回车，接下来安装Opcache：

# ./addons.sh install opcache

回车，再回车。

当出现 “Opcache installed successfully, enjoy it!” 字样时，即表示安装成功。

接着安装Memcached：

# ./addons.sh install memcached

回车，选择2，回车，再回车。

当出现 “Memcached installed successfully, enjoy it!” 字样时，即表示安装成功。

此时，可以删除之前下载的lnmp1.5安装包，以节省空间。

# rm -rf /root/lnmp1.5-full.tar.gz

回车即可。

接下来就可以添加域名安装WordPress了。

#7添加域名 / 虚拟主机
请提前做好域名解析（建议至少提前半个小时），例如：

域名添加A记录

可直接略过解释说明，查看详细操作步骤。

同时，建议在操作前先开启443端口：CentOS7防火墙中开启相关端口

添加域名：

# lnmp vhost add

回车，提示输入域名：

# seoimo.com

回车，提示是否添加多个域名：

# y

回车，博主习惯绑定带 www 的域名：

# www.seoimo.com

回车，显示网站目录。默认 /home/wwwroot/seoimo.com 即可。

若是绑定二级域名，必须输入完整的目录路径。例如：
将 tools.seoimo.com 绑定到网站根目录下的 tools 文件夹，则应输入：

/home/wwwroot/seoimo.com/tools

回车。博主习惯不需要日志记录，但建议你开启（y）：

# n

会车后，输入站长邮箱。

继续回车，提示数据库名和数据库用户名是否保持一致。

# y

回车，输入 root 用户的数据库密码（不会显示，在#6搭建LAMP环境中设置好的）。

回车，输入数据库名，自行设置。例如：

# sjk_seoimo

回车，设置数据库密码。例如：

# sjkmmseoimo

是否需要开启SSL/HTTPS访问，推荐开启：

# y

这里使用免费的Let's Encrypt证书，所以选择 2 。
安装WordPress程序
以下的步骤想必应该很熟悉，和带Cpanel或DirectAdmin面板安装WordPress过程比较类似。只不过，在面板上操作是可视化的，比较直观。而在这里是通过命令执行的，非可视。只要输入命令时细心点，一般是不会出问题的。

首先，进入添加的域名目录：

# cd /home/wwwroot/seoimo.com

回车。然后浏览器中打开WordPress中文站点，下载最新的程序压缩包：

# wget https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz

回车。等待下载完之后，解压压缩包：

# tar -zxvf wordpress-4.9.4-zh_CN.tar.gz

回车。

接下来，将解压出来的wordpress文件夹内全部文件移动到当前的域名目录下（别忘了后面的.）。

# mv wordpress/* .

回车。然后，可以选择删掉空文件夹wordpress及源程序（可选）。

# rm -rf wordpress wordpress-4.9.4-zh_CN.tar.gz

回车，搞定。

为避免因权限的问题导致安装出错，比如wp-config.php无法创建、需要提供FTP用户密码以及主题和插件不能更新等，建议赋予网站根目录文件的可写权限。

# chmod -R 755 /home/wwwroot && chown -R www /home/wwwroot
好了，打开博客网址进行最后的安装吧！（记得要提前设置好域名解析）

安装SSL证书后，建议打开HTTPS开头的网址安装。如已经使用HTTP安装，需要把HTTP替换为HTTPS网址。

搭建WordPress博客

至此，在VPS上通过搭建LAMP环境安装WordPress博客已经大功告成了。
