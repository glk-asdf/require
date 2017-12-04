[MD学习](http://blog.csdn.net/kaitiren/article/details/38513715) 




## require 运行过程
### 自定义关键字，和依赖
```javascript
    require.config({
    baseUrl:"/resources/js",
    paths : {
        "a":"a",
        "b":"b/b",
        "c":"c/c.js",
    },
    shim:{
        "a":{
            "deps":["b","c","d/d","css!"+base+"/resources/css/a.css",'css!b/css/b.css']
        },
    }

})
```
        引入js （a 和 b），执行 a 和 b，将返回的值放入一个数组中，arr
        执行回调函数callBack，callBack.apply({},arr);
        a 和 b 可以为自定义关键字,也可以是路径（基于baseUrl）

```javascript
    require(["a","b","c","d/d"],function(a){
        console.log(a);
    })
```

### 写可引入js

```javascript
    define(["a","b"], function (a) {
        console.log(1);
        return 2;
    })
```

        普通插件 a.js
        插件：写一个对象，含有多个组件
        组件：写一个函数，传入参数 opt 返回一个对象各种使用方法
        函数：写一个构造函数，new 一个对象，并返回这个对象
        构造函数：先将参数opt，并入局部的默认配置相中，写一个公共方法 publicMethod 和私有方法 privateMethod ，然后将公共方法 publicMethod 并入 this 供外部使用

```javascript
    var a = {
        a:function(opt){
            function aInstance(){
                var defaultOption = {
                    a:1,
                    b:2
                }
                var $this = this;
                opt = $.extend(defaultOption,opt);
                var publicMethod = {
                    geta:function(){},
                    seta:function(){},
                }
                var privateMethod = {
                    a:function(){},
                    b:function(){},
                }
                $.extend($this,publicMethod);
            }
            return new aInstance();
        }
    }
```

### require的引用 a.js

```javascript
    define(["a","b"], function (a) {
        return {
            a:function(opt){
                function aInstance(){
                    var defaultOption = {
                        a:1,
                        b:2
                    }
                    var $this = this;
                    opt = $.extend(defaultOption,opt);
                    var publicMethod = {
                        geta:function(){},
                        seta:function(){},
                    }
                    var privateMethod = {
                        a:function(){},
                        b:function(){},
                    }
                    $.extend($this,publicMethod);
                }
                return new aInstance();
            }
        };
    })
```