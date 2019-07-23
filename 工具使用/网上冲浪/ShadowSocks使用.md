# PAC文件及user-rule文件的语法规则

那么，当一个网站被墙，如何添加到PAC里面让其能够正常访问呢？在Shadowsocks里面，可以有如下两个方式：

## 1. 添加到 pac.txt 文件中

编辑 pac.txt 文件，模仿里面的一些URL通配符，再添加一个，例如"||ip138.com", ，注意不要忘记了 , 半角逗号，那么意思就是所有 ip138.com域名下的网址都将走Shadowsocks代理，打开ip138可以看到IP已经变成Shadowsocks所用的国外代理了![alt](https://6xyun.cn/upload/2018/02/6itqdert1cindrscltes8rm441.png)

## 2. 添加到 user-rule.txt 文件中

编辑 user-rule.txt 文件，这里和 pac.txt 文件语法不完全相同，user-rule文件中，每一行表示一个URL通配符，但是通配符语法类似。例如添加一行||ip138.com^ ，然后记得***右键小飞机-PAC-从GFWList更新本地PAC***，打开ip138可以看到IP已经变成Shadowsocks所用的国外代理了![alt](https://6xyun.cn/upload/2018/02/3vjn79vf2uit5prto2asqoa39s.png)注意末尾不要忘记 ^ 符号，意思是要么在这个符号的地方结束，要么后面跟着?,/等符号。

# 语法

自定义代理规则的设置语法与GFWlist相同，语法规则如下：

- 通配符支持。比如 *.example.com/* 实际书写时可省略 * ， 如.example.com/ ， 和 *.example.com/* 效果一样
- 正则表达式支持。以 \ 开始和结束， 如 [\w]+://example.com\
- 例外规则 @@ ，如 @@*.example.com/* 满足 @@ 后规则的地址不使用代理
- 匹配地址开始和结尾 | ，如 |http://example.com 、 example.com| 分别表示以 http://example.com 开始和以 example.com 结束的地址
- || 标记，如 ||example.com 则 http://example.com 、https://example.com 、 ftp://example.com 等地址均满足条件
- 注释 ! 。 如 !我是注释

更多user-rule.txt语法规则，可以参考AdBlockPlus过滤规则https://adblockplus.org/en/filter-cheatsheet