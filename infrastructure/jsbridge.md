# jsBridge

## javascript调用Native

### 注入API

注入 API 方式的主要原理是，通过 WebView 提供的接口，向 JavaScript 的 Context（window）中注入对象或者方法，让 JavaScript 调用时，直接执行相应的 Native 代码逻辑，达到 JavaScript 调用 Native 的目的。

调用方式
```javascript
    window.postBridgeMessage(message);
```

### 拦截URL SCHEME

URL SCHEME是一种类似于url的链接，是为了方便app直接互相调用设计的，形式和普通的 url 近似，主要区别是 protocol 和 host 一般是自定义的。

例如: `qunarhy://hy/url?url=ymfe.tech`，`protocol`是`qunarhy`，`host` 则是`hy`。

拦截 URL SCHEME 的主要流程是：Web 端通过某种方式（例如 iframe.src）发送 URL Scheme 请求，之后 Native 拦截到请求并根据 URL SCHEME（包括所带的参数）进行相关操作。

#### 缺点
1. 使用 iframe.src 发送 URL SCHEME会有url长度隐患
2. 创建请求需要一定的耗时，比注入API的方式调用同样的功能，耗时较长。
3. 如果使用localtion.href跳转，连续调用时很容易造成丢失。


## Native调用javascript

Native 调用 JavaScript，其实就是执行拼接 JavaScript 字符串，从外部调用 JavaScript 中的方法，因此 JavaScript 的方法必须在全局的 window 上。