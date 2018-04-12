## Fetch API

> 个人理解fetch是一种可以代替XMLHttpRequest的一种网络请求方案。能够做之前使用XMLHttpRequest所做的所有事。而且可以做XMLHttpRequest所不能做的事。比如跨域请求。      


> MDN 对于fetch API的概念：fetch提供了request和response（以及其他与网络请求有关）的通用定义。是的fetch可以被运用与很多场景：比如`service workers`、`Cache API`、或则其他处理请求或响应的方式。，甚至任何一种你需要在自己的程序中生成的方式。

#### Fetch的使用

> `fetch()`接收一个参数，一个`url`，无论请求是否成功，fetch返回一个promise对象，resolve对应response.同时也可以传入第二个参数`init`。init是一个request参数。包含比如`headers`、`body`、`method`等。用于指定请求的方式、数据等。具体使用方法[Request](https://developer.mozilla.org/zh-CN/docs/Web/API/Request)或[Fetch使用](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)。


tips:*目前fetch并不是所有浏览器都支持。所以对于低版本浏览器，需要自己做兼容。或者使用`fetch polyfill`。集体兼容情况请看[fetch兼容情况](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)*

#### Fetch使用中注意

> 如果对于fetch的使用不是很熟悉的话，可能会在使用中遇到很多的问题。具体问题只有在使用中慢慢体会了。

说说我在看法过程中遇到的问题：
1. 如果使用post提交数据的话，肯定是要定义`fetch`的第二个参数`init`。在定义`headers`一定要根据你的提交数据使用正确的`Conten-Type`。如果指定错误，可能会遇到两种情况。（1）提交方式非你指定的方式。我在进行表单提交时，指定的`method:POST`但是提交的时候却是以`GET`方式提交的。（2）第二种就是在后台你无法就收到你提交的数据。后台无法解析。所以一定要使用正确的`Content-Type`。
2. 如果你的表单提交有文件上传，也就是使用`<input type="file"/>`的话。需要使用`formData`提交数据（formdata的定义和用法请查看[fromdata](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)）。那么你的`enctype`就需要设置成`multipart/form-data`，而后台接收如果使用的是node（其他语言不熟悉，google），`bodypase`并不能解析。就需要用到`multer`一类的中间件了。
3. 如果你的表单中没有文件的提交。请不要使用`formData`。如果使用，那么你需要设置`enctype:multipart/form-data`，你后台还是不得不使用`multer`，否则`req.body`教会为空，这会得不偿失。如果使用`enctype:x-www-form-urlencoded`。那么你的`req.body`的到的数据看起来回事这要`{ '------WebKitFormBoundaryScyHIVaj7rPAA7IL\r\nContent-Disposition: form-data; name': '"username"\r\n\r\ntiandd\r\n------WebKitFormBoundaryScyHIVaj7rPAA7IL\r\nContent-Disposition: form-data; name="password"\r\n\r\nsdfdsf\r\n------WebKitFormBoundaryScyHIVaj7rPAA7IL--\r\n' }`，这样是不能处理的。            

[详解传送门](http://tech.dianwoda.com/2016/11/10/formbiao-dan-de-jin-jie-xue-xi/)


fetch 虽然是未来的趋势。但是坑也挺多的。使用还要多注意。



