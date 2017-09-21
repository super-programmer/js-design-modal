# 浅谈js设计模式
`所有的设计模式结果都是为了提高生产和后期维护效率！！！`
## 常见设计模式
1.单例模式

>单例就是保证一个类`只有一个实例`，实现的方法一般是先判断实例存在与否，如果存在直接返回，如果不存在就创建了再返回，
这就确保了一个类只有一个实例对象。在JavaScript里，单例作为一个命名空间提供者，从全局命名空间里提供一`个唯一的访问点来访问该对象`
***
```javascript
var createDiv = (function(){
    var div;
    return function(){
        if(!div){
            div = document.createElement('div');
            div.style.width = '100px';
            div.style.height = '100px';
            div.style.background = 'red';
            div.style.marginBottom = '10px';
            document.body.appendChild(div);
        }
    }
})();
createDiv();
createDiv();
createDiv();
```

2.简单工厂模式

>简单工厂模式是由一个方法来决定到底要创建哪个类的实例, 而这些实例经常都拥`有相同的接口`.
这种模式主要用在所实例化的类型在编译期并不能确定， 而是在执行期决定的情况。
就像现实生活中的自动贩卖机，
用户只要点击对应产品的按钮就可以获得对应产品.

***
```javascript
function Flight() {
        getDom('facipt').value = 'This is Flight';
    }
    function Hotel() {
        getDom('facipt').value = 'This is hotel';
    }
    function User() {
        this.shopCart = [];
    }
    var productFactory = (function () {
        var productFactories = {
            "flight": function () {
                return new Flight();
            },
            "hotel": function () {
                return new Hotel();
            }
        };

        return {
            createProduct: function (productType) {
                return productFactories[productType]();
            }
        }
    })();
    User.prototype = {
        constructor: User,
        order: function (productType) {
            this.shopCart.push(productFactory.createProduct(productType));
        }
    }
    var productEnums = {
        flight: "flight",
        hotel: "hotel"
    }
    var user = new User();
    getDom('facbtn1').onclick = function () {
        user.order(productEnums.flight);
    };
    getDom('facbtn2').onclick = function () {
        user.order(productEnums.hotel);
    }
```

3.观察者模式
>观察者模式是:一个观察者（observer，也叫订阅者：subscriber）订阅一个可观察对象（observable，也叫发布者：publisher），期待发生些有趣的事。
观察者也可以退订。这种行为依赖你对模式的具体实现。对于观察者有两个基本的方式可以获取数据：push和pull。push方式是一但发生变化，发布者会立即触发订阅者的提醒事件。
pull方式是只要订阅都觉得有必要，随时可以检查发布者的变化。

*publisher -> push subscriber
***
```javascript
var Observable = function() {
    this.subscribers = [];
}

Observable.prototype = {
    subscribe: function(callback) {
        // 大多数情况下，你会想要检查订阅者数组里是否已经存在这个回调(callback)了。
        // 不过我们现在没有必要关注这些旁枝末节的东西。
        this.subscribers.push(callback);
    },
    unsubscribe: function(callback) {
        var i = 0,
            len = this.subscribers.length;

        // 遍历数组，如果找到这个回调(callback)，就删除它。
        for (; i < len; i++) {
            if (this.subscribers[i] === callback) {
                this.subscribers.splice(i, 1);
                // 一旦我们找到了，就没必要继续执行后面的代码，直接return。
                return;
            }
        }
    },
    publish: function(data) {
        var i = 0,
            len = this.subscribers.length;

        // 遍历整个订阅者数组，执行每一个回调。
        for (; i < len; i++) {
            this.subscribers[i](data);
        }
    }
};

var Observer = function (data) {
    console.log(data);
}
// 这里是具体的用法
observable = new Observable();
observable.subscribe(Observer);
observable.publish('我们发布了！');
```
*subscriber -> push publisher
***
```javascript
Observable = function() {
    this.status = "constructed";
}
Observable.prototype.getStatus = function() {
    return this.status;
}

Observer = function() {
    this.subscriptions = [];
}
Observer.prototype = {
    subscribeTo: function(observable) {
        this.subscriptions.push(observable);
    },
    unsubscribeFrom: function(observable) {
        var i = 0,
            len = this.subscriptions.length;

        // 遍历数组，如果找到这个发布者(observable)，就删除它。
        for (; i < len; i++) {
            if (this.subscriptions[i] === observable) {
                this.subscriptions.splice(i, 1);
                // 一旦我们找到了，就没必要继续执行后面的代码，直接return。
                return;
            }
        }
    },
    doSomethingIfOk: function() {
        var i = 0;
            len = this.subscriptions.length;

        // 遍历subscriptions确定每个元素的状态是否变成了ok，
        // 如果是ok的话就处理
        for (; i < len; i++) {
            if (this.subscriptions[i].getStatus() === "ok") {
                // 做些处理，因为observable已经变成我们想要的状态
            }
        }
    }
}

var observer = new Observer(),
    observable = new Observable();
observer.subscribeTo(observable);

// 因为状态并没有改变所以什么都不会发生
observer.doSomethingIfOk();

// 把状态变为ok，现在你才会处理
observable.status = "ok";
observer.doSomethingIfOk();
```



