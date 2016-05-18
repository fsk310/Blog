##1.单例模式

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