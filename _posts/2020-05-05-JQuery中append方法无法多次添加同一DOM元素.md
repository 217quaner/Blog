# JQuery中append方法无法多次添加同一DOM元素
## 原因
append()方法在jQuery中是通过appendChild来操作dom的，因此你后面写的不是不起作用，而是在原地移动，因为就一个对象，你再怎么写也是一个。
```js
function () {
    return this.domManip(arguments, function (elem) {
        if (this.nodeType === 1 || this.nodeType === 11 || this.nodeType === 9) {
            var target = manipulationTarget(this, elem);
            target.appendChild(elem);
        }
    });
}
```

## 解决方法
1. 将DOM元素写进append()中
```js
$("xxx").append( " <p class='xxx'>·····</p> " );
```

2. 使用克隆方法
```js
$("xxx").append($("td").clone());
```
