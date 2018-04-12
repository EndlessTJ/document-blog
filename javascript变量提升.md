## javascript变量提升
> 首先我们来看两个例子      

        var a = 1;
        function test(){
            if(!a) var a = 10
            alert(a)
        }
        test() //结果是10
        
> 是不是回有一些困惑，那么我们再来看下一个例子      

        var a = 1;
        function test(){
            a = 10
            return;
            function a (){}
        }
        test()
        alert(a)//结果是1
> 咦，是不是很神奇，当然如果你了解过javascript的scoping，那就另当别论了。这就是js的scoping

> 在JavaScript中，一个作用域(scope)中的名称(name)有以下四种：

1. 语言自身定义(Language-defined): 所有的作用域默认都会包含this和arguments。

2. 函数形参(Formal parameters): 函数有名字的形参会进入到函数体的作用域中。

3. 函数声明(Function decalrations): 通过function foo() {}的形式。

4. 变量声明(Variable declarations): 通过var foo;的形式。


> 上面的两个例子在其实经历了这样的过程      

        var a = 1;
        function test(){
            if(!a) var a = 10
            alert(a)
        }
        test() //结果是10
        /*
        *函数里实际被解释成这样
        */
        function test(){
            var a;
            if(!a){
                a = 10
            }
            alert(a)
        }
        //在javascript的函数作用域中，声明变量和函数，会被自动提升当函数的顶部，如果是变量的话，只是提升变量，不会提升值，而如果是函数的话，整个函数都会被提升。所以就会出现刚才上面的那种情况。
        /*
        *再来看一看第二个例子
        **/
        //第二个例子会被解释成
          var a = 1;
        function test(){
            function a (){}
            a = 10
            return;
        }
        test()
        alert(a)//结果是1
        //在函数中a被重新定义了，所以实际在函数中a = 10.而这个a只在函数中有作用
        
详情可以参考[这篇博客](http://www.cnblogs.com/betarabbit/archive/2012/01/28/2330446.html)