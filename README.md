# unbound.conf

![unbound.conf](https://i.imgur.com/zoFgNsM.png)

[DNSCrypt](https://github.com/jedisct1/dnscrypt-proxy) is a piece of lightweight software that everyone should use to boost online privacy and security. It works by encrypting all DNS traffic between the user and DNS resolver, preventing any spying, spoofing or man-in-the-middle attacks.

[Unbound](https://www.unbound.net/) is a validating, recursive, and caching DNS resolver. 

[NirCmd](http://www.nirsoft.net/utils/nircmd.html) is a small command-line utility that allows you to do some useful tasks without displaying any user interface.

[Wget](https://www.gnu.org/software/wget/) is a free software package for retrieving files using HTTP, HTTPS and FTP, the most widely-used Internet protocols.

## 使用方法

### Linux 用户

* 可参考[树莓派安装unbound和dnscrypt-proxy全记录](https://github.com/CNMan/unbound.conf/issues/13)进行配置，大同小异。

### Windows 用户

* 下载 [DNSCrypt 32-bit](https://download.dnscrypt.org/dnscrypt-proxy/dnscrypt-proxy-win32-full-1.9.4.zip)或[DNSCrypt 64-bit](https://download.dnscrypt.org/dnscrypt-proxy/dnscrypt-proxy-win64-full-1.9.4.zip)、[Unbound](http://unbound.net/downloads/unbound-1.6.0.zip)、[NirCmd 32-bit](http://www.nirsoft.net/utils/nircmd.zip)或[NirCmd 64-bit](http://www.nirsoft.net/utils/nircmd-x64.zip)、[Wget 32-bit](http://nebm.ist.utl.pt/~glopes/wget/wget-1.11.4-x86.zip)或[Wget 64-bit](http://nebm.ist.utl.pt/~glopes/wget/wget-1.11.4-x64.zip)

* 解压`dnscrypt-proxy-win32-full-1.9.4.zip`或`dnscrypt-proxy-win64-full-1.9.4.zip`到`E:\Program Files\DNSCrypt\`；解压`unbound-1.6.0.zip`到`E:\Program Files\unbound\`；解压`nircmd.zip`或`nircmd-x64.zip`到`C:\Windows\System32\` ；解压`wget-1.11.4-x86.zip`或`wget-1.11.4-x64.zip`到`C:\Windows\System32\`（注：解压各zip包里的`.exe`和`.dll`到目标文件夹即可，其他文件用不到）

![filelist](https://i.imgur.com/uJTvBjd.png)

* 下载[配置文件](https://github.com/CNMan/unbound.conf/archive/master.zip)，解压`unbound`目录中的文件到`E:\Program Files\unbound\`，解压`dnscrypt-proxy`目录中的文件到`E:\Program Files\DNSCrypt\`，解压`localdns.cmd`到`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\`(Vista+)或`C:\Documents and Settings\All Users\Start Menu\Programs\Startup`(XP-)

![conffilelist](https://i.imgur.com/SrdL07C.png)

* 根据实际情况修改[localdns.cmd](https://github.com/CNMan/unbound.conf/blob/master/localdns.cmd)、[localdns-update.cmd](https://github.com/CNMan/unbound.conf/blob/master/localdns-update.cmd)和[unbound.conf](https://github.com/CNMan/unbound.conf/blob/master/unbound/unbound.conf#L4)中的相关安装目录

* 若无法完成以上步骤，可下载傻瓜包[localdns.zip](https://dl.cnlic.com/localdns.zip)，必须解压`localdns`目录到C盘根目录(否则需要参照上面说明修改配置和批处理中的绝对路径)

* 若有其他占用53、8953和9999端口的软件，请停用(依次执行如下命令：`netstat -ano | findstr PORT`、`tasklist | findstr ProcessID`、`taskkill /IM ImageName /F`)

![port](https://i.imgur.com/wb72J7A.png)

* 运行`localdns.cmd`启动DNSCrypt和Unbound

* 参考[dig安装与使用](https://github.com/CNMan/unbound.conf/issues/6)，测试53和9999端口的解析是否正常(`dig @127.0.0.1 www.google.com -p 53`、`dig @127.0.0.1 www.google.com -p 9999`)

* 在网络设置里面把DNS服务器设置成127.0.0.1

![localdns](https://i.imgur.com/4WN9qit.png)

* 运行`localdns-update.cmd`更新配置，亦可手动下载更新`unbound.bogus-nxdomain.China.conf`、`unbound.forward-zone.China.conf`、`unbound.local-zone.adhosts.China.conf`、`unbound.local-zone.hosts.conf`、`dnscrypt-blacklist-domains.txt`、`dnscrypt-blacklist-ips.txt`

## 说明

* 可对局域网或公网服务(需搭建在国内服务器上，否则仍有污染)

* 主要配置文件在北京时间每天5:09、13:09、21:09与上游项目同步更新一次；傻瓜包[localdns.zip](https://dl.cnlic.com/localdns.zip)每天5:30更新一次

* 常用hosts域名配置在`unbound.local-zone.hosts.conf`，配置说明可参考[Unbound+DNSCrypt双保险防DNS污染及劫持](https://goo.gl/IG3K27)

* 广告域名和恶意软件域名配置在`dnscrypt-blacklist-domains.txt`，由DNSCrypt负责过滤；国内广告域名和恶意软件域名配置在`unbound.local-zone.adhosts.China.conf`，由Unbound负责过滤

* 国内运营商bogus nxdomain ip配置在`unbound.bogus-nxdomain.China.conf`，[烦请帮忙收集各地运营商的bogus nxdomain ip](https://github.com/CNMan/unbound.conf/issues/11)

* 国内域名配置在`unbound.forward-zone.China.conf`，默认由114.114.114.114和223.5.5.5解析，可自行替换为当地运营商的DNS服务器，如果你有其他国内域名需要添加，请直接向[dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)项目反馈。

* 其他域名由监听在9999端口的DNSCrypt负责解析

* 修改unbound配置文件后，请先检查有没有错误配置，再重启unbound并刷新DNS解析缓存

```
cd /d "E:\Program Files\unbound\"
unbound-checkconf unbound.conf
unbound-control -c unbound.conf reload
ipconfig /flushdns
```

![unbound-control](https://i.imgur.com/FWjHwjh.png)

## 参考资料

[http://unbound.nlnetlabs.nl/documentation/unbound.conf.html](http://unbound.nlnetlabs.nl/documentation/unbound.conf.html)

[http://unbound.nlnetlabs.nl/svn/trunk/doc/example.conf.in](http://unbound.nlnetlabs.nl/svn/trunk/doc/example.conf.in)

[https://github.com/jedisct1/dnscrypt-proxy/raw/master/dnscrypt-proxy.conf](https://github.com/jedisct1/dnscrypt-proxy/raw/master/dnscrypt-proxy.conf)

## 致谢

* [https://github.com/racaljk/hosts](https://github.com/racaljk/hosts)

* [https://github.com/koala0529/adhost](https://github.com/koala0529/adhost)

* [https://github.com/vokins/yhosts](https://github.com/vokins/yhosts)

* [https://github.com/felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)

## dnscrypt-proxy 命令行参数

```
dnscrypt-proxy 1.9.4

Compilation date: Jan 21 2017
Support for plugins: present
Support for ldns-based plugins: present
Support for the XChaCha20-Poly1305 cipher: present

Options:

  -R    --resolver-name=...
  -a    --local-address=...
  -E    --ephemeral-keys
  -K    --client-key=...
  -L    --resolvers-list=...
  -l    --logfile=...
  -m    --loglevel=...
  -X    --plugin=...
  -N    --provider-name=...
  -k    --provider-key=...
  -r    --resolver-address=...
  -n    --max-active-requests=...
  -u    --user=...
  -t    --test=...
  -T    --tcp-only
  -e    --edns-payload-size=...
  -I    --ignore-timestamps
  -V    --version
  -h    --help
        --install
        --install-with-config-file=...
        --reinstall
        --reinstall-with-config-file=...
        --uninstall
        --service-name=...
```

## unbound 命令行参数

```
usage:  unbound [options]
	start unbound daemon DNS resolver.
-h	this help
-c file	config file to read instead of C:\Program Files (x86)\Unbound\service.conf
	file format is described in unbound.conf(5).
-d	do not fork into the background.
-v	verbose (more times to increase verbosity)
-w opt	windows option: 
   	install, remove - manage the services entry
   	service - used to start from services control panel
Version 1.6.0
linked libs: event winsock (it uses WSAWaitForMultipleEvents), OpenSSL 1.0.2j  26 Sep 2016
linked modules: dns64 validator iterator
BSD licensed, see LICENSE in source package for details.
Report bugs to unbound-bugs@nlnetlabs.nl
```

## unbound-checkconf 命令行参数

```
Usage:	unbound-checkconf [file]
	Checks unbound configuration file for errors.
file	if omitted C:\Program Files (x86)\Unbound\service.conf is used.
-o option	print value of option to stdout.
-f 		output full pathname with chroot applied, eg. with -o pidfile.
-h		show this usage help.
Version 1.6.0
BSD licensed, see LICENSE in source package for details.
Report bugs to unbound-bugs@nlnetlabs.nl
```

## unbound-control 命令行参数

```
Usage:	unbound-control [options] command
	Remote control utility for unbound server.
Options:
  -c file	config file, default is C:\Program Files (x86)\Unbound\service.conf
  -s ip[@port]	server address, if omitted config is used.
  -q		quiet (don't print anything if it works ok).
  -h		show this usage help.
Commands:
  start				start server; runs unbound(8)
  stop				stops the server
  reload			reloads the server
  				(this flushes data, stats, requestlist)
  stats				print statistics
  stats_noreset			peek at statistics
  status			display status of server
  verbosity <number>		change logging detail
  log_reopen			close and open the logfile
  local_zone <name> <type>	add new local zone
  local_zone_remove <name>	remove local zone and its contents
  local_data <RR data...>	add local data, for example
				local_data www.example.com A 192.0.2.1
  local_data_remove <name>	remove local RR data from name
  dump_cache			print cache to stdout
  load_cache			load cache from stdin
  lookup <name>			print nameservers for name
  flush <name>			flushes common types for name from cache
  				types:  A, AAAA, MX, PTR, NS,
					SOA, CNAME, DNAME, SRV, NAPTR
  flush_type <name> <type>	flush name, type from cache
  flush_zone <name>		flush everything at or under name
  				from rr and dnssec caches
  flush_bogus			flush all bogus data
  flush_negative		flush all negative data
  flush_stats 			flush statistics, make zero
  flush_requestlist 		drop queries that are worked on
  dump_requestlist		show what is worked on by first thread
  flush_infra [all | ip] 	remove ping, edns for one IP or all
  dump_infra			show ping and edns entries
  set_option opt: val		set option to value, no reload
  get_option opt		get option value
  list_stubs			list stub-zones and root hints in use
  list_forwards			list forward-zones in use
  list_insecure			list domain-insecure zones
  list_local_zones		list local-zones in use
  list_local_data		list local-data RRs in use
  insecure_add zone 		add domain-insecure zone
  insecure_remove zone		remove domain-insecure zone
  forward_add [+i] zone addr..	add forward-zone with servers
  forward_remove [+i] zone	remove forward zone
  stub_add [+ip] zone addr..	add stub-zone with servers
  stub_remove [+i] zone		remove stub zone
		+i		also do dnssec insecure point
		+p		set stub to use priming
  forward [off | addr ...]	without arg show forward setup
				or off to turn off root forwarding
				or give list of ip addresses
  ratelimit_list [+a]		list ratelimited domains
		+a		list all, also not ratelimited
Version 1.6.0
BSD licensed, see LICENSE in source package for details.
Report bugs to unbound-bugs@nlnetlabs.nl
```

## dig 命令行参数

```
Usage:  dig [@global-server] [domain] [q-type] [q-class] {q-opt}
            {global-d-opt} host [@local-server] {local-d-opt}
            [ host [@local-server] {local-d-opt} [...]]
Where:  domain    is in the Domain Name System
        q-class  is one of (in,hs,ch,...) [default: in]
        q-type   is one of (a,any,mx,ns,soa,hinfo,axfr,txt,...) [default:a]
                 (Use ixfr=version for type ixfr)
        q-opt    is one of:
                 -4                  (use IPv4 query transport only)
                 -6                  (use IPv6 query transport only)
                 -b address[#port]   (bind to source address/port)
                 -c class            (specify query class)
                 -f filename         (batch mode)
                 -i                  (use IP6.INT for IPv6 reverse lookups)
                 -k keyfile          (specify tsig key file)
                 -m                  (enable memory usage debugging)
                 -p port             (specify port number)
                 -q name             (specify query name)
                 -t type             (specify query type)
                 -u                  (display times in usec instead of msec)
                 -x dot-notation     (shortcut for reverse lookups)
                 -y [hmac:]name:key  (specify named base64 tsig key)
        d-opt    is of the form +keyword[=value], where keyword is:
                 +[no]aaflag         (Set AA flag in query (+[no]aaflag))
                 +[no]aaonly         (Set AA flag in query (+[no]aaflag))
                 +[no]additional     (Control display of additional section)
                 +[no]adflag         (Set AD flag in query (default on))
                 +[no]all            (Set or clear all display flags)
                 +[no]answer         (Control display of answer section)
                 +[no]authority      (Control display of authority section)
                 +[no]badcookie      (Retry BADCOOKIE responses)
                 +[no]besteffort     (Try to parse even illegal messages)
                 +bufsize=###        (Set EDNS0 Max UDP packet size)
                 +[no]cdflag         (Set checking disabled flag in query)
                 +[no]class          (Control display of class in records)
                 +[no]cmd            (Control display of command line)
                 +[no]comments       (Control display of comment lines)
                 +[no]cookie         (Add a COOKIE option to the request)
                 +[no]crypto         (Control display of cryptographic fields in records)
                 +[no]defname        (Use search list (+[no]search))
                 +[no]dnssec         (Request DNSSEC records)
                 +domain=###         (Set default domainname)
                 +[no]dscp[=###]     (Set the DSCP value to ### [0..63])
                 +[no]edns[=###]     (Set EDNS version) [0]
                 +ednsflags=###      (Set EDNS flag bits)
                 +[no]ednsnegotiation (Set EDNS version negotiation)
                 +ednsopt=###[:value] (Send specified EDNS option)
                 +noednsopt          (Clear list of +ednsopt options)
                 +[no]expire         (Request time to expire)
                 +[no]fail           (Don't try next server on SERVFAIL)
                 +[no]header-only    (Send query without a question section)
                 +[no]identify       (ID responders in short answers)
                 +[no]ignore         (Don't revert to TCP for TC responses.)
                 +[no]keepopen       (Keep the TCP socket open between queries)
                 +[no]mapped         (Allow mapped IPv4 over IPv6)
                 +[no]multiline      (Print records in an expanded format)
                 +ndots=###          (Set search NDOTS value)
                 +[no]nsid           (Request Name Server ID)
                 +[no]nssearch       (Search all authoritative nameservers)
                 +[no]onesoa         (AXFR prints only one soa record)
                 +[no]opcode=###     (Set the opcode of the request)
                 +[no]qr             (Print question before sending)
                 +[no]question       (Control display of question section)
                 +[no]rdflag         (Recursive mode (+[no]recurse))
                 +[no]recurse        (Recursive mode (+[no]rdflag))
                 +retry=###          (Set number of UDP retries) [2]
                 +[no]rrcomments     (Control display of per-record comments)
                 +[no]search         (Set whether to use searchlist)
                 +[no]short          (Display nothing except short
                                      form of answer)
                 +[no]showsearch     (Search with intermediate results)
                 +[no]split=##       (Split hex/base64 fields into chunks)
                 +[no]stats          (Control display of statistics)
                 +subnet=addr        (Set edns-client-subnet option)
                 +[no]tcp            (TCP mode (+[no]vc))
                 +timeout=###        (Set query timeout) [5]
                 +[no]trace          (Trace delegation down from root [+dnssec])
                 +tries=###          (Set number of UDP attempts) [3]
                 +[no]ttlid          (Control display of ttls in records)
                 +[no]ttlunits       (Display TTLs in human-readable units)
                 +[no]unknownformat  (Print RDATA in RFC 3597 "unknown" format)
                 +[no]vc             (TCP mode (+[no]tcp))
                 +[no]zflag          (Set Z flag in query)
        global d-opts and servers (before host name) affect all queries.
        local d-opts and servers (after host name) affect only that lookup.
        -h                           (print help and exit)
        -v                           (print version and exit)
```
