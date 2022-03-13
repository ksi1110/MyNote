#### DNS2IP，通过DNS解析找到IP地址

1. ping，非权威解答，ping testfire.net
2. nslookup
3. dig
   1. 常规用法，dig testfire.net
   2. 指定DNS服务器解析，dig @8.8.8.8 testfire.net
   3. 获取域名解析的详细过程，dig +trace testfire.net
4. dnsenum，解析域名时会自动检测域传送漏洞
   1. 常规用法：dnsenum testfire.net
5. 通过站长工具解析域名获取IP

#### CDN内容分发网络

本意是进行节点缓存，使网站访问速度加快。一般情况下无法获得目标网站的真实IP

#### IP查询，获得IP地址的物理位置

1. IP查询，站长之家工具
2. 同IP网站，使用同一个ip的网站，站长之家工具
3. IP whois 查询
4. IP2location，查询IP地址经纬度，通过经纬度查询物理地址
