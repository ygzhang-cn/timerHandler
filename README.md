# jQuery timerHandler
最近遇到个全站Ajax加载维护的项目，更新了下公司库里的timerHandler，增加支持基于Dom节点移除的自动销毁已绑定定时器的事件机制

## 使用方法
1、页面引入插件，也支持AMD/CMD方式加载
传统页面引入方式，使用 $.timerHandler 注册管理定时器

NODE/AMD/CMDD方式加载引入，使用 define Name/ exports module Name 注册管理定时器, 默认为 timerHandler 

2、开始使用

```javascript
//注册一个名为'mytimer1'的定时器，每5秒运行一次
$.timerHandler('mytimer1').time('5s').call(function(count){
    //this => timerHandler Obj
    console.log('mytimer1 runing : '+count);
}).start();


//注册一个名为'mytimer2'的定时器，每500毫秒运行一次，运行10次后销毁
$.timerHandler('mytimer2').time('500ms').limit(10).call(function(count){            
    //this => timerHandler Obj
    console.log('mytimer2 runing : '+count);
}).callStopEnd(function(){
    console.log('mytimer2 stop');
}).start();

//注册一个名为'mytimer3'的定时器，每1秒运行一次，并将其绑定在DOM ID为 bindDomID 的节点上，当此Dom节点移除后自动销毁
$('body').append('<div id="bindDomID">注册一个名为'mytimer3'的定时器，每1秒运行一次，并将其绑定在DOM ID为 bindDomID 的节点上，当此Dom节点移除后自动销毁</div>')
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

3、定时器对象缓存数据注册在jQuery($)对象上 $.timerHandler
