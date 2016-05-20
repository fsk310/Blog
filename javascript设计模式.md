##单例模式

###单例模式的对象只会被创建一个，例如webqq的登录窗口，在正常情况下有且仅有一个。
```javascript
var ProxySingleton = (function  () {
    var instance;
    return function  (obj) {        // 这里使用了闭包的一些特性
        if(!instance){
            instance = new Singleton(obj);
        }
        return instance;
    }
})();
```

###利用高阶函数把函数当做对象传入
```
var getSingle = function  (fn) {
    var result;
    return function  () {
        return result || (result = fn.apply(this,arguments));
    }
};
```

## 享元模式

##一种用于性能优化的模式，适用于系统中因为创建了大量类似的对象而导致内存不足或占用过高。对象池是一类典型的享元模式的例子。
```javascript
var toolTipFactory = (function () {
    var toolTipPool  = [];   // toolTip 对象池
    return {
        create : function () {
            if(toolTipPool.length === 0){
                var div = document.createElement('div');
                document.body.appendChild(div);
                return div;
            } else {
                return toolTipPool.shift(); //shift a dom
            }
        },
        recover : function (tooltipDom) {
            return toolTipPool.push(tooltipDom);  
        }
    }
})();
```
