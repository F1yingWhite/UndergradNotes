# 作用域
没有通过var声明,会自动变为全局变量
```javascript
var name;
consloe.log(name);//undefined

//var声明的函数表达式不会被提前
myFunction()//报错
var myFUnction() =function(){}

//函数中声明函数会有作用域炼,像上级函数查找
var tep = new Date();

function f() {
    console.log(tep);//var tep会放到f最前面,所以输出dunefined
    if (false) {
        var tep = "helloworld";
    }
}
```

```js
let a=1//只有块级作用域,而且不存在变量提前而且不允许变量名重复
```