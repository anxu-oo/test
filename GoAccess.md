---
title: goaccess日志分析
categories: 
- linux
tags:
- nginx
---

<font size=2>GoAccess 是一个采用c语言开发的来分析统计Web服务器的访问日志的工具，速度非常快，可即时生成自定义的统计报表，基于命令行操作。</font>

<font size=2>比如，我有上万台机器的话，每台机器都会产生日志。如果其中有 10 台机器中的程序报了一个 error，你不可能在登录上去看吧。而且，登录系统都是需要临时审批权限的。所以，我们要有更高效的日志管理办法。</font>

<font size=2>有人可能会想到 ELK，是的，ELK 确实牛。但是对于一些小公司来说 ELK 就太笨重了。而且针对 Nginx 或者 Apache，我们需要其他的日志可视化显示和监控系统，那么 GoAccess 就是一个不错的选择！</font>

<!--more-->

###### 下载软件

```
 wget http://tar.goaccess.io/goaccess-1.3.tar.gz
```

###### 解压软件

```
tar -xf goaccess-1.3.tar.gz
tar -xf GeoLite2-City_20200421.tar.gz -d /usr/local/src/
cd goaccess-1.3/
```

###### 安装依赖编译

```
yum -y install glib2 glib2-devel ncurses ncurses-devel GeoIP GeoIP-devel
./configure --enable-utf8 --enable-geoip=/usr/local/src/GeoLite2-City_20200421/GeoLite2-City.mmdb
make && make install
```

 GoAccess 拥有多个配置选项。获取完整的最新配置选项列表，请运行：./configure —help。

–enable-debug：使用调试标志编译且关闭编译器优化。
–enable-utf8：宽字符支持。依赖 Ncursesw 模块。
–enable-geoip=<legacy|mmdb>：地理位置支持。依赖 MaxMind GeoIP 模块。 legacy 将使用原始 GeoIP 数据库。 mmdb 将使用增强版 GeoIP2 数据库。
–enable-tcb=<memhash|btree>：Tokyo Cabinet 存储支持。 memhash 将使用 Tokyo Cabinet 的内存哈希数据库。btree 将使用 Tokyo Cabinet 的磁盘 B+Tree 数据库。
–disable-zlib：禁止在 B+Tree 数据库上使用 zlib 压缩。
–disable-bzip：禁止在 B+Tree 数据库上使用 bzip2 压缩。
–with-getline：使用动态扩展行缓冲区用来解析完整的行请求，否则将使用固定大小(4096)的缓冲区。
–with-openssl：使 GoAccess 与其 WebSocket 服务器之间的通信能够支持 OpenSSL。

###### 更改系统语言

vim /etc/locale.conf
LANG=zh_CN.UTF8
#系统语言必须是英文时
LANG="zh_CN.UTF-8" bash -c "goaccess  --geoip-database=/usr/local/src/GeoLite2-City_20200421/GeoLite2-City.mmdb-d -f /data/ApplicationLog/nginxlog/logs_all/hwyhw_smarket_net_cn.log -a -p /etc/goaccess.conf -o /data/goaccess/report.html --real-time-html --daemonize"

###### 指定日志格式

vim /usr/local/goaccess-1.3/config/goaccess.conf
time-format %H:%M:%S
date-format %d/%b/%Y
log-format %h %^[%d:%t %^] %^ "%r" %s %b "%R" %^ %^ "%u" %^ %^ %^:%T %^:%^ %^ %^:%^

######  测试生成日志

```
goaccess -d -f 日志名字 -a -p /usr/local/goaccess-1.3/config/goaccess.conf > test.html
#搭建一个nginx或者下载到本地看啦
LANG="zh_CN.UTF-8" bash -c "goaccess -d -f /data/ApplicationLog/nginxlog/logs_all/hwyhw_smarket_net_cn.log -a -p /etc/goaccess.conf -o /data/goaccess/report.html --real-time-html --daemonize"
```

###### 命令行模式快捷键

```
F1   主帮助页面
F5   重绘主窗口
q    退出
1-15 跳转到对应编号的模块位置
o    打开当前模块的详细视图
j    当前模块向下滚动
k    当前模块向上滚动
s    对模块排序
/    在所有模块中搜索匹配
n    查找下一个出现的位置
g    移动到第一个模块顶部
G    移动到最后一个模块底部
```

###### 日志格式

> %x 匹配 time-format 和 date-format 变量的日期和时间字段。用于使用时间戳来代替日期和时间两个独立变量的场景。
> %t 匹配 time-format 变量的时间字段。
> %d 匹配 date-format 变量的日期字段。
> %v 根据 canonical 名称设定的服务器名称(服务区或者虚拟主机)。
> %e 请求文档时由 HTTP 验证决定的用户 ID。
> %h 主机(客户端IP地址，IPv4 或者 IPv6)。
> %r 客户端请求的行数。这些请求使用分隔符(单引号，双引号)引用的部分可以被解析。否则，需要使用由特殊格式说明符(例如：%m, %U, %q 和 %H)组合格式去解析独立的字段。
> 注意: 既可以使用 %r 获取完整的请求，也可以使用 %m, %U, %q and %H 去组合你的请求，但是不能同时使用。
> %m 请求的方法。
> %U 请求的 URL。#GET /events/20200415/js/jquery.common.js
> 注意: 如果查询字符串在 %U中，则无需使用 %q。但是，如果 URL 路径中没有包含任何查询字符串，则你可以使用 %q 查询字符串将附加在请求后面。
> %q 查询字符串。
> %H 请求协议。
> %s 服务器回传客户端的状态码。
> %b 回传客户端的对象的大小。
> %R HTTP 请求的 "Referer" 值。
> %u HTTP 请求的 "UserAgent" 值。
> %D 处理请求的时间消耗，使用微秒计算。
> %T 处理请求的时间消耗，使用带秒和毫秒计算。
> %L 处理请求的时间消耗，使用十进制数表示的毫秒计算。
> %^ 忽略此字段。
> %~ 继续解析日志字符串直到找到一个非空字符(!isspace)。
> ~h 在 X-Forwarded-For (XFF) 字段中的主机(客户端 IP 地址，IPv4 或者 IPv6)。
> $remote_addr   客户端 IP 地址
> $remote_user   客户端用户名称
> $time_local       本地时间
> $host    访问域名
> "$request   "请求的URI和HTTP协议
> '$status  状态码
> $body_bytes_sent  用于记录发送客户端的文件主体内容大小
> "$http_referer"      用于记录是从哪个页面链接访问过来的
> "$http_user_agent"  客户端浏览器
> $http_x_forwarded_for    用于记录真实IP地址
> $hostname 客户端名字
> $request_time 整个请求的总时间
> $upstream_response_time     请求过程中，upstream响应时间（php）
> $remote_addr, $http_x_forwarded_for 记录客户端IP地址
> $remote_user 记录客户端用户名称
> $request 记录请求的URL和HTTP协议
> $status 记录请求状态
> $body_bytes_sent 发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod_log_config里的“%B”参数兼容。
> $bytes_sent 发送给客户端的总字节数。
> $connection 连接的序列号。
> $connection_requests 当前通过一个连接获得的请求数量。
> $msec 日志写入时间。单位为秒，精度是毫秒。
> $pipe 如果请求是通过HTTP流水线(pipelined)发送，pipe值为“p”，否则为“.”。
> $http_referer 记录从哪个页面链接访问过来的
> $http_user_agent 记录客户端浏览器相关信息
> $request_length 请求的长度（包括请求行，请求头和请求正文）。
> $request_time 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
> $time_iso8601 ISO8601标准格式下的本地时间。
> $time_local 通用日志格式下的本地时间。
> 阿里云cdn日志格式：
> [15/Apr/2020:19:59:53 +0800] 223.242.160.44 - 5 "/events/20200415/index.html" "GET /events/20200415/js/jquery.common.js" 200 42 2555 HIT "Mozilla/5.0 (Linux; Android 10; MAR-AL00 Build/HUAWEIMAR-AL00; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/67.0.3396.87 XWEB/1169 MMWEBSDK/200301 Mobile Safari/537.36 MMWEBID/9831 MicroMessenger/7.0.13.1640(0x27000D37) Process/tools NetType/WIFI Language/zh_CN ABI/arm64 WeChat/arm64" "application/javascript"
> log-format [%d:%t %^] %h %^ %L %^ "%R%U" %s %^ %b %^ "%u" "%^"
> 现如今web中间件一般都会有代理/cdn等，这样服务器获取的就不是真实ip了，可以在http{} 中加上
> map $http_x_forwarded_for  $clientRealIp {
>         ""      $remote_addr;#如果$http_x_forwarded_for为空则等于$remote_addr
>         ~^(?P<firstAddr>[0-9\.]+),?.*$  $firstAddr;#如果有值时等于$firstAddr
>         }
> 把日志中的remote_addr换成clientREalip

