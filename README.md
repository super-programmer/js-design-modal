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


