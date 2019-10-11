# Linux禁止ping以及开启ping的方法

​	Linux默认是允许Ping响应的，系统是否允许Ping由2个因素决定的：A、内核参数，B、防火墙，需要2个因素同时允许才能允许Ping，2个因素有任意一个禁Ping就无法Ping。

## 具体的配置方法如下：

 **A、内核参数设置**

###   1、允许PING设置

​        A.临时允许PING操作的命令为：#echo 0 >/proc/sys/net/ipv4/icmp_echo_ignore_all

​         B.永久允许PING配置方法。

​              **/etc/sysctl.conf** 中增加一行

　　            **net.ipv4.icmp_echo_ignore_all=1**

​          如果已经有net.ipv4.icmp_echo_ignore_all这一行了，直接修改=号后面的值即可的（0表示允许，1表示禁止）。

​          修改完成后执行**sysctl -p**使新配置生效。

​        ![QQ？？？20150309171941.png](http://img01.taobaocdn.com/tfscom/TB1hPCjHpXXXXa8XpXXXXXXXXXX.png)

### 2、禁止Ping设置     

​         A.临时禁止PING的命令为：#echo 1 >/proc/sys/net/ipv4/icmp_echo_ignore_all     

​       B.永久允许PING配置方法。

​              **/etc/sysctl.conf** 中增加一行

　　            **net.ipv4.icmp_echo_ignore_all=0**

​         如果已经有net.ipv4.icmp_echo_ignore_all这一行了，直接修改=号后面的值即可的。（0表示允许，1表示禁止）

​         修改完成后执行**sysctl -p**使新配置生效。

​         ![QQ？？？20150309173326.png](http://img01.taobaocdn.com/tfscom/TB1.AKXHpXXXXcQaXXXXXXXXXXX.png)

 

 

​    **B、防火墙设置（**注：此处的方法的前提是内核配置是默认值，也就是没有禁止Ping）

​     这里以Iptables防火墙为例，其他防火墙操作方法可参考防火墙的官方文档。

​     1、允许PING设置      

​        iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT

​        iptables -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT

​       或者也可以临时停止防火墙操作的。

​        service iptables stop

 

​     2、禁止PING设置

​        iptables -A INPUT -p icmp --icmp-type 8 -s 0/0 -j DROP