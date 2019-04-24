# 性能监控

## 统计指标

### 白屏时间（first Paint Time）
用户从打开页面开始到页面开始有东西呈现为止

### 首屏时间
用户浏览器首屏内所有内容都呈现出来所花费的时间

## #用户可操作时间(dom Interactive)
用户可以进行正常的点击、输入等操作，默认可以统计domready时间，因为通常会在这时候绑定事件操作

### 总下载时间
页面所有资源都加载完成并呈现出来所花的时间，即页面 onload 的时间


## 统计方法

### 白屏时间

白屏时间=开始渲染时间(首字节时间+HTML下载完成时间)+头部资源加载时间

#### 如何获取

#####  chrome 高版本：

`window.chrome.loadTimes().firstPaintTime loadTimes`获取的结果

计算公式为

`(chrome.loadTimes().firstPaintTime - chrome.loadTimes().startLoadTime)*1000`


##### 其他浏览器

大部分浏览器没有特定函数，必须想其他办法来监测。仔细观察 WebPagetest 视图分析发现，白屏时间出现在头部外链资源加载完附近，因为浏览器只有加载并解析完头部资源才会真正渲染页面。

基于此我们可以通过获取头部资源加载完的时刻来近似统计白屏时间。尽管并不精确，但却考虑了影响白屏的主要因素：首字节时间和头部资源加载时间（HTML下载完成时间非常微小）。

开始渲染时间
`window.performance.timing.navigationStart`

头部资源加载时间
```html
<!DOCTYPE HTML>
<html>
    <head>
    <script>
        const endTime = +new Date;
    </script>
    </head>
</html>
```

因此白屏时间为
```
var firstPaintTime = endTime - performance.timing.navigationStart
```

### 用户可操作时间

用户可操作为所有DOM都解析完毕的时间，默认可以统计domready时间，因为通常会在这时候绑定事件操作。

对于使用了模块化异步加载的 JS 可以在代码中去主动标记重要 JS 的加载时间，这也是产品指标的统计方式。

计算公式:
```
performance.timing.domInteractive - performance.timing.navigationStart
```

### 总下载时间
默认可以统计onload时间，这样可以统计同步加载的资源全部加载完的耗时。

如果页面中存在很多异步渲染，可以将异步渲染全部完成的时间作为总下载时间。


计算公式:
```
performance.timing.loadEventStart - performance.timing.navigationStart
```


## 参考资料
[前端性能统计指标](https://juejin.im/post/5b5ed5046fb9a04fd343a8c7)

[7天打造前端性能监控](http://fex.baidu.com/blog/2014/05/build-performance-monitor-in-7-days/)