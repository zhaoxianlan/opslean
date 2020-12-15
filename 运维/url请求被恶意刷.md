参考博文:
[ 封禁IP的办法 (自动化脚本封ip, 根据瞬时的请求总量,或者根据日志中某段时间的总量)](http://siwei.me/blog/posts/linux-nginx-ip-ip)

[博客](https://www.cnblogs.com/huligong1234/p/7189426.html)



1. nginx的层面封IP 
2.  linux server的层面封IP

1、添加crontab

```
*/2 * * * * sh /opt/check_black_ip.sh
```

2、过滤IP，追加配置

```
tail -n10000 /usr/local/nginx/logs/access.log | awk '{print $1,$7}' | grep send_login_token | awk '{print $1}' | sort | uniq -c | sort -rn | awk '{if($1>30)print "deny "$2";"}' > /usr/local/nginx/conf/blacklist.append.conf
/usr/local/nginx/sbin/nginx -s reload
------------------------------------------


```

3、增加nginx 配置

```
http{
    # ....
    include blacklist.conf;
}
```

