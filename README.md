# extension-calculator
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031022511681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)

# 介绍

谷歌浏览器是开发人员和普通用户最喜欢的浏览器之一。我在所有设备上都使用了Google Chrome浏览器，它可以帮助我同步书签，浏览器历史记录，密码管理器等等。

对于台式机，除了可以在Internet上浏览以外，您还可以做很多事情。您可以测试您的网页和全部。通过使用扩展程序，谷歌浏览器变得更加强大。

因此，今天，我们将研究如何使用HTML，CSS和JavaScript创建您的第一个Google Chrome扩展程序。

# 设置

## 要求

Chrome扩展入门的要求很少。列表在这里：

 - Google Chrome扩展程序（用于测试）
 - 文本编辑器（我更喜欢VS Code，您可以根据需要使用其他编辑器）
 - 有关HTML，CSS和JavaScript的基础知识

# Chrome扩展程序

我们将为第一个Chrome扩展程序开发一个计算器应用程序。如果您知道如何为网络应用程序制作计算器，那么本教程将对您来说很容易。现在，您只需要知道“如何设置扩展名？”即可。

## manifest.json

每个应用程序都需要一个清单---一个描述该应用程序的JSON格式文件，名为`manifest.json`。此文件将帮助您的应用管理权限，存储，清单版本，登录页面，名称，图标等。

这是清单的代码

```css
{
    "manifest_version" : 2,
    "name" : "Calculator",
    "version" : "1.0",
    "description" : "Calculate Anywhere",
    "icons" : {
        "128" : "img/icons128.png",
        "48" : "img/icons48.png",
        "16" : "img/icons16.png"
    },
    "browser_action" : {
        "default_icon" : "img/icons16.png",
        "default_popup" : "popup.html"
    },
    "content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self'"
}
```

## 解释

 - manifest_version：您正在定义要使用的清单的版本。我们目前使用的是2，但最近Google推出了版本3。
 - name：这是您的应用程序的名称。目前，我们称其为“计算器”。
 - description：顾名思义，您将在此处描述扩展名。很少有描述扩展名的句子就足够了。现在，我们将其命名为“随时随地计算”。
 - icons：您需要为扩展程序的图标提供src。您需要提供图标大小不同的来源。
 - browser_actions：我们使用browser_actions将扩展名放置在工具栏上，该工具栏位于地址栏的右边。浏览器操作具有图标，工具提示，徽章和弹出窗口。
 - default_icon：图标图像的来源。
 - default_popup：它是扩展程序登录页面的源。它必须为HTML格式。您可以根据自己的名字命名。对我来说，它是“popup.html”。
 - content_security_policy：声明允许chrome扩展视为其他功能。我使用`eval()`函数来计算我不建议用于商业目的的方程式。您可以使用单独的函数进行计算。

## popup.html，popup.js和style.css

这里我不会随意更改HTML，CSS和JavaScript部分。不过您可以自由编写

### popup.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="style.css">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Wanghao | Calculator</title>
</head>

<body>
    <div class="inputButton">
        <input type="text" id="textField">
    </div>
        <div class="inputButton">
            <div class="rowField">
                <button id="7b" class="small">7</button>
                <button id="8b" class="small">8</button>
                <button id="9b" class="small">9</button>
                <button id="+b" class="small">+</button>
            </div>

            <div class="rowField">
                <button id="4b" class="small">4</button>
                <button id="5b" class="small">5</button>
                <button id="6b" class="small">6</button>
                <button id="-b" class="small">-</button>
            </div>

            <div class="rowField">
                <button id="1b" class="small">1</button>
                <button id="2b" class="small">2</button>
                <button id="3b" class="small">3</button>
                <button id="*b" class="small">*</button>
            </div>

            <div class="rowField">
                <button id="0b" class="medium">0</button>
                <button id="/b" class="small">/</button>
            </div>

            <div class="rowField">
            <button id="submit" class="large">Calculate</button>
            </div>
        </div>
    <script src="popup.js"></script>
</body>

</html>

```

### style.css

```css
* {
    margin: 0;
    padding: 0;
}

body{
    background-color: #2b3252;
}

#textField{
    border: none ;
    border-radius: 2rem;
    padding: 0.25em;
    color:  #2b3252
}

.inputButton{
    margin : 0.5rem;
    max-width: 90%;
}

.rowField{
    display: flex;
    justify-content: space-between;
}

.small,
.medium,
.large{
    margin: 0.2rem;
    background-color: #fce77d;
    border: none ;
    border-radius: 2em;
    color:  #2b3252
}

.small:hover,
.medium:hover,
.large:hover{
    background-color: #ef5455
}

.small{
    width : 30%
}

.medium{
    width : 100%
}

.large{
    width:100%
}
```

### popup.js

```javascript
document.getElementById("submit").addEventListener("click", calculate)
document.getElementById("7b").addEventListener("click", () => (des(7)))
document.getElementById("8b").addEventListener("click", () => (des(8)))
document.getElementById("9b").addEventListener("click", () => (des(9)))
document.getElementById("4b").addEventListener("click", () => (des(4)))
document.getElementById("5b").addEventListener("click", () => (des(5)))
document.getElementById("6b").addEventListener("click", () => (des(6)))
document.getElementById("1b").addEventListener("click", () => (des(1)))
document.getElementById("2b").addEventListener("click", () => (des(2)))
document.getElementById("3b").addEventListener("click", () => (des(3)))
document.getElementById("0b").addEventListener("click", () => (des(0)))
document.getElementById("+b").addEventListener("click", () => (des("+")))
document.getElementById("/b").addEventListener("click", () => (des("/")))
document.getElementById("-b").addEventListener("click", () => (des("-")))
document.getElementById("*b").addEventListener("click", () => (des("*")))

function calculate(){
    var input = document.getElementById("textField").value
    const value = eval(input)
    document.getElementById("textField").value=value
}

function des(val){

    document.getElementById("textField").value+=val
}
```

您可以在我的Github存储库中查看整个代码。

[https://github.com/wanghao221/extension-calculator](https://github.com/wanghao221/extension-calculator)

或者Gitee存储库

[https://gitee.com/haiyongcsdn/extension-calculator](https://gitee.com/haiyongcsdn/extension-calculator)

## 在Chrome中安装扩展程序

为了进行检查，我们首先将代码下载解压到我们的电脑中。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310232330744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)


首先，访问 chrome://extensions 打开扩展程序管理器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310232557367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)


单击加载解已解压的扩展程序按钮。出现一个文件对话框。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021031023272938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)



在文件对话框中，选择`extension-calculator`（目录包含manifest.json的文件夹）。除非出现错误对话框，否则您现在已经安装了该应用程序。如下图就是成功了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310232825347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)
成功安装后，在右上角可以看到，如果没有，点开右边像拼图一样的东西，里面就可以找到

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310233010257.png)


## 最后注

至此我们自制的计算器就完成了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210310233145942.png)

如果出现错误，请检查您的代码并尝试解决它。如果您遇到任何麻烦，可以在这里与我联系。

## 相关内容

 - [勇敢的兔子疯狂奔跑小游戏](https://mp.weixin.qq.com/s/h-F1Yx3LAMVKsvDpTTKjbg)
- [用HTML实现简单的下雪特效](http://mp.weixin.qq.com/s?__biz=Mzg5OTU2NTQ4MQ==&mid=2247484065&idx=1&sn=17a958a2a8df8458bf316fac55f865f3&chksm=c0501067f7279971add268cff499833f173c5d7368040c8c902da698eb72bdba23d10aca385a#rd)
 - [基于HTML/CSS/JS的动态元素周期表](https://blog.csdn.net/qq_44273429/article/details/114296024)
 - [基于HTML/CSS/JS的爱吹风的狮子小游戏](https://blog.csdn.net/qq_44273429/article/details/113792583)
 - [100个最常问的JavaScript面试问答](https://blog.csdn.net/qq_44273429/article/details/114240168)
 - [java五子棋小游戏含免费源码](https://mp.weixin.qq.com/s?__biz=Mzg5OTU2NTQ4MQ==&amp;mid=2247483719&amp;idx=1&amp;sn=8c86ff28a782c838a184a4050c695d26&amp;chksm=c0501381f7279a97df8f4467203e8de05ef7fd34f1d9be51a11c591c8e37e244cb010c55afda&token=925595149&lang=zh_CN#rd)
 - [一个炫光效果的酷炫登录表单](https://blog.csdn.net/qq_44273429/article/details/113797520)

感谢您阅读至最后❤️这是您的勋章
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309222038491.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)


最后，不要忘了❤️或📑支持一下哦
