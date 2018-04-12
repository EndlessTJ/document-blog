
> 这篇文章将向你解释如何使用`Node.js`的*Crypto*模块对你的密码进行加盐`hash`。在这里，我们将不会对不懂的密码存储方式进行详细的比较。我们将要做的是知道在`Node.js`中使用加盐`hash`在进行密码存储的机制。放心，这是最好的存储密码的方式，在没有出现其他更好的方法之前。

## 这是什么技术

加盐是这样一直技术：将用户输入的密码和一个随机的字符串（这个字符串就是盐）通过hash结合，通过hash算法生成一个hash值，然后将结果存储在数据库中。

## 为什么要进行加盐Hash
因为相同密码的hash值是相同的，所以那时很容易通过lookup tables和rainbow tables来破解密码（lookup tables 和rainbow talbes是一些人预先破解了一些常见的密码然后存储起来以供其他人使用的表格）。如果有两个或者更多的用户拥有相同的*密码hash值*，将会让攻击者更加容易预测密码。加盐是为了让有相同*密码hash值*得这种情况减少。 如果你的盐（随机字符串）够长，那么这种几率将会几乎为零。

## 进行实践

> 在实践中你将会进行以下这些操作

#### 创建并保存密码
1. 获取用户密码
2. 生成一个*盐*（随机字符串）
3. 将*盐*和用户密码进行结合
4. 使用加密算法对结合后的字符串进行hash加密
5. *将hash的结果作为密码*进行存储，同时将*盐*和*密码*一起存储。

#### 验证用户密码
1. 验证用户名并从数据库中获取*hash后的密码*以*及盐*
2. 将*用户输入的密码*和对应用户已经存储的*盐*进行结合
3. 使用和创建用户是相同的hash加密算法对结合字符串进行hash加密
4. 将hash加密后的密码和数据控存储的密码进行比较




## 让我们来看看实际代码

首先需要引入`crypto`这个模块。你不需要单独去下载它，因为他是Node.js的内置模块，只需引入即可。

```
'use strict'
var crypto = require('crypto')


```

#### 创建一个function用来生成盐

```
/**
 * generates random string of characters i.e salt
 * @function
 * @param {number} length - Length of the random string.
 */
 
 
 var genRandomString = function(length){
     return crypto.randomBtytes(Math.ceil(length/2))
                .toString('hex') /**转成十六进制*/
                .slice(0,length);/**返回指定长度字符串*/
 }
 
```

#### 将密码和盐一起进行hash加密

使用一种你在读这篇文章时代的一种合适的加密hash算法，因为伴随着计算能力的发展，加密hash技术也会发展。所以如果你使用就得hash加密算法是存在很大风险的。这里我们使用`sha512`

```
/**
 * hash password with sha512.
 * @function
 * @param {string} password - List of required fields.
 * @param {string} salt - Data to be validated.
 */
 
 var sha512 = function(password, salt){
     var hash = crypto.createHamc('sha512', salt); /**使用sha512算法进行hash*/
     hash.update(password)
     var value = hash.digest('hex')
     return {
         salt:salt,
         passwordHash:value
     }
  }

```


下面我们将创建一个函数使用让面的的函数生成一个hash值，作为用户密码存储在数据库中。


```

function saltHashPassword(userpassword){
    var salt = genRandowString(16) /**生成一个长度为16个字符的盐*/
    var passwordData = sha512(userpassword, salt);
    console.log( console.log('UserPassword = '+userpassword);
    console.log('Passwordhash = '+passwordData.passwordHash);
    console.log('nSalt = '+passwordData.salt);)
}

saltHashPassword('MYPASSWORD');
saltHashPassword('MYPASSWORD');
```

注意，我们当用`saltHashPassword`两次使用相同的参数，只是想要展示使用相同密码的两次hash加密的结果是不同的。

下面让我们来看看完整的代码展示



```
'use strict';
var crypto = require('crypto');

/**
 * generates random string of characters i.e salt
 * @function
 * @param {number} length - Length of the random string.
 */
var genRandomString = function(length){
    return crypto.randomBytes(Math.ceil(length/2))
            .toString('hex') /** convert to hexadecimal format */
            .slice(0,length);   /** return required number of characters */
};

/**
 * hash password with sha512.
 * @function
 * @param {string} password - List of required fields.
 * @param {string} salt - Data to be validated.
 */
var sha512 = function(password, salt){
    var hash = crypto.createHmac('sha512', salt); /** Hashing algorithm sha512 */
    hash.update(password);
    var value = hash.digest('hex');
    return {
        salt:salt,
        passwordHash:value
    };
};

function saltHashPassword(userpassword) {
    var salt = genRandomString(16); /** Gives us salt of length 16 */
    var passwordData = sha512(userpassword, salt);
    console.log('UserPassword = '+userpassword);
    console.log('Passwordhash = '+passwordData.passwordHash);
    console.log('nSalt = '+passwordData.salt);
}

saltHashPassword('MYPASSWORD');
saltHashPassword('MYPASSWORD');

```

在运行这段代码，你将会的得到两个不同的hash值（这里就不做展示了，有兴趣的可以自己运行看看）


## 结论

无论你做任何需要存储用户密码的web应用，它都非常容易出现问题，所以这里我们强烈推荐你使用加盐hash来对用户密码进行处理，这将有效的提高密码存储的安全性。



[英文原文地址](https://ciphertrick.com/2016/01/18/salt-hash-passwords-using-nodejs-crypto/)