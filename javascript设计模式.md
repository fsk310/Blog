##单例模式

####单例模式的对象只会被创建一个，例如webqq的登录窗口，在正常情况下有且仅有一个。
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

####利用高阶函数把函数当做对象传入
```
var getSingle = function  (fn) {
    var result;
    return function  () {
        return result || (result = fn.apply(this,arguments));
    }
};
```

## 享元模式

####一种用于性能优化的模式，适用于系统中因为创建了大量类似的对象而导致内存不足或占用过高。对象池是一类典型的享元模式的例子。
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

##责任链模式

####使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系，将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。
```javascript
var Chain = function (fn) {
    this.fn = fn;
    this.successor = null;
}
Chain.prototype.setNextSuccessor = function (successor) {
    return this.successor = successor;
}
Chain.prototype.passRequest = function () {
    var ret = this.fn.apply(this.arguments);
    if(ret === 'nextSuccessor'){
        return this.successor && this.successor.passRequest.apply(this.successor,arguments)
    }
    return ret;
}
```

##中介者模式

####中介者模式的作用就是解除对象与对象之间的紧耦合关系。增加一个中介者对象后，所以的相关对象都通过中介者对象来通信，而不是互相引用，所以当一个对象发生改变时，只需要通知中介者对象即可。中介者使各对象之间耦合松散，而且可以独立地改变它们之间的交互。中介者模式使网状的多对多关系变成了相对简单的一对多关系。
```javascript
var director = (function () {
    var operations = {};
    //这里需要所有对象之间进行沟通的属性。通过该中介者处理所有对象的通信，使各个对象之间耦合松散
    operations.test = function (str) {
        alert(str);
    }
    var ReceiveMessage = function () {
        var message = [].prototype.shift.call(arguments);
        operations[message].apply(this,arguments);
    };
    return{
        ReceiveMessage : ReceiveMessage
    }
});
```
