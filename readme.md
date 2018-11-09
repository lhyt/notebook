# 移动端
## 混合开发
### h5与hybrid通信
H5调用NA
>
```
平台     | 方法                     | 备注

Android | shouldOverrideUrlLoading | scheme拦截方法
Android | addJavascriptInterface   | API
Android | onJsAlert()、onJsConfirm()、onJsPrompt（）
IOS     | 拦截URL 
IOS(UIwebview) | JavaScriptCore | API方法，IOS7+ 支持
IOS(WKwebview) | window.webkit.messageHandlers | APi方法，IOS8+支持
```

NA调用H5方法
> 
```
平台     | 方法                     | 备注

Android | loadurl()                |
Android | evaluateJavascript()   | Android 4.4 +
IOS(UIwebview) | stringByEvaluatingJavaScriptFromString
IOS(UIwebview) | JavaScriptCore | IOS7.0+
IOS(WKwebview) | evaluateJavaScript:javaScriptString | iOS8.0+
```

URL拦截 & 执行JS是 安卓和IOS比较通用且兼容性较好的方案

H5和NA通信方面，最简单直接的思路是：NA拦截H5的URL获取消息（一般是通过修改iframe的src来实现 ①），经过业务处理，NA执行JS（在H5侧提前注册好的全局方法③）回调通知H5

```
<body>
    <div class="content">XXXXX</div>
</body>
  
<script>
    // ① 注册全局函数,以便端调用
    window.setAllContent = function(){
         
    }
 
    // ② 通用方法函数
    var sendschema = function(action,param){
        let tempnode = document.createElement('iframe');
        tempnode.src = "bdnews://"+action+param;
    }
 
    // ③ H5逻辑开始 运行函数
    document.addEventListener("DOMContentLoaded",function(){
        sendschema('load_finish');
    },false);
</script>
```
