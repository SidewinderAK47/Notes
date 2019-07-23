## 问题执行命令的时候 要加./ 呢

解答: 其中linux 程序的执行过程解答说明： 

```shell
aaa.sh
```


如果是相对路径，会去$PATH所在的路径找有没有执行程序。所以这么使用之前需要在环境变量中指定程序路径。

```shell
./aaa.sh 
```

如果是绝对路径，就可直接执行。

## SUID&SGID的使用方法

例如程序文件的属主是root，那么执行该程序的用户就将暂时获得root账户的权限。sgid与suid类似，只是[执行程序](https://www.baidu.com/s?wd=执行程序&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)时获得的是文件属组的权限。
你可以看一下passwd这个命令程序的权限设置，它就是设置了suid权限的。
设置方法为：

```bash
chmod u+s filename (suid)
chmod g+s filename (sgid)
```

发现设置test程序的SUID位之后，test进程的有效用户ID等于文件所有者的UID（gkh的uid为500），有效用户组ID还是等于实际用户组ID（0）。这样程序就可以访问只有gkh才能访问的资源了。
