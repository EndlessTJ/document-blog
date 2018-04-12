## 关于JWT

> jwt包含三个部分，分别是`header` `playload` `signature`      
> `header`一般包含的是token的类型和加密的方式
```
    {
        type: 'jwt',
        alg: 'sha256'
    }
```
> 然后以`base64`的格式进行编码，编码后大概这个样子`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9`;     

>`playload`一般包含一些信息，申明。标准的注册申明一般是这样的（只是建议，不做强制要求）      
> - iss:jwt的签发者
> - sub:jwt所面向的用户
> - aud:jwt的接收一方
> - exp:jwt的过期时间，必须大于签发时间
> - nbf:定义在什么时间之前该jwt不可用
> - iat:jwt的签发时间
> - jti:jwt的唯一身份标识，主要用来作为一次性token，从而回避重放攻击      

*注意：你也可以添加其他的内容*
> 它也是json格式
```
    {
        iss:'TJ',
        sub:'user',
        admin:true
    }
```
> 然后也是要用`base64`格式进行编码后，然后发送。        

> `signature`是`header`和`playload`这两项的`base64`编码连接，然后用设定的`secret`加盐，使用header中的加密方式,这里我们用的是`sha256`进行hash加密。这就生成了特定的`token`,然后我们就可以使用这个`token`进行权限验证了。