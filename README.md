# jQuery timerHandler
jQuery timerHandler 定时器插件支持动态注册管理，支持已ms/s/m/h时间设置间隔周期时间、暂停、限制运行次数及自动销毁机制
插件主页：https://github.com/ygzhang-cn/timerHandler/

2014.7.12 插件入库

2014.7.27 改善插件支持AMD/CMD方式使用

2014.7.28 改善插件支持Node环境使用

2018.1.30 最近遇到个全站Ajax加载维护的项目，更新了下公司库里的timerHandler，增加支持基于Dom节点移除的自动销毁已绑定定时器的事件机制

## 使用方法
1、引入插件，传统方式页面引入
使用 $.timerHandler 管理注册定时器

2、AMD/CMD/Node方式加载
使用 define Name/ exports module Name 管理注册定时器, 默认为 timerHandler 

3、接口方法
注：定时器对象缓存数据注册在jQuery($)对象上 $.timerHandler

|名称|参数|返回值|说明|
|--------|--------|--------|--------|
|timerHandler(value)|string|timerHandler Obj|注意一个名称为value的定时器，并返回一个定时器对象，如已存在返回已注册定时器|
|timerHandler.time(value)|string or integer|this|设置间隔周期时间|
|timerHandler.limit(value)|integer|this|设置重复次数，默认0不限制，具体数值N 限制运行N次后自动销毁|
|timerHandler.bindDom(value)|jQuery selector string|this|绑定注册到Dom节点，适用于Ajax加载的脚本定时器，将会监听Dom移除事件，Dom移除后，绑定的定时器也将销毁，如不指定定时器绑定在全局|
|timerHandler.call(callback)|function|this|定时器回调主函数|
|timerHandler.callStart(callback)|function|this|开始之前的回调函数|
|timerHandler.callStartEnd(callback)|function|this|开始之后的回调函数|
|timerHandler.callPause(callback)|function|this|暂停之前的回调函数|
|timerHandler.callPauseEnd(callback)|function|this|暂停之后的回调函数|
|timerHandler.callStop(callback)|function|this|停止(销毁)之前的回调函数|
|timerHandler.callStopEnd(callback)|function|this|停止(销毁)之后的回调函数|
|timerHandler.start()|无|this|启动定时器|
|timerHandler.pause()|无|this|暂停定时器|
|timerHandler.stop()|无|bool|停止(销毁)定时器|

4、开始使用

```javascript
//注册一个名为'mytimer1'的定时器，每5秒运行一次
var myTimer=$.timerHandler('mytimer1').time('5s').call(function(count){
    //this => timerHandler Obj
    console.log('mytimer1 runing : '+count);
}).start();
//myTimer.pause();//暂停定时器运行
//myTimer.start();//定时器重新启动
//myTimer.call(callback);//重新注册定时器回调主函数
//myTimer.stop();//停止(销毁)定时器

//注册一个名为'mytimer2'的定时器，每500毫秒运行一次，运行10次后销毁
$.timerHandler('mytimer2').time('500ms').limit(10).call(function(count){            
    //this => timerHandler Obj
    console.log('mytimer2 runing : '+count);
}).callStopEnd(function(){
    console.log('mytimer2 stop');
}).start();

//注册一个名为'mytimer3'的定时器，每1秒运行一次，并将其绑定在DOM ID为 bindDomID 的节点上，当此Dom节点移除后自动销毁
$('body').append('<div id="bindDomID">注册了一个名为mytimer3的定时器，每1秒运行一次，并将其绑定在DOM ID为 bindDomID 的节点上，当此Dom节点移除后自动销毁（此节点10秒后自动移除）</div>')
$.timerHandler('mytimer3').time('1s').bindDom('#bindDomID').call(function(count){            
    //this => timerHandler Obj
    console.log('mytimer3 runing : '+count);
}).callStopEnd(function(){
    console.log('mytimer3 stop');
}).start();
//10秒后移除DOM ID为 bindDomID 的节点
$.timerHandler('bindDomIDRemove').time('10s').limit(1).call(function(count){            
    //this => timerHandler Obj
    console.log('dom#bindDomID is remove');
    $('#bindDomID').remove();
}).start();
```


