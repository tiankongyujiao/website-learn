## ajax和jsonp
ajax大家都不陌生了，它的作用很简单，就是基本取代了form表单提交数据，它的好处就是不刷新页面（个人理解，不同意见请指正）。在我开始接触前端的时候，ajax就已经很流行了，当然那时还有form的使用场景，但是后面基本把form取代了，提高用户体验。  
 
ajax默认是异步的，在异步派发xmlHttpRequest请求后，控制权马上就被返回到浏览器。在响应数据之前可以在界面上加loadding...  
通过AJAX异步技术，可以在客户端脚本与web服务器交互数据的过程中使用XMLHttpRequest对象来完成HTTP请求(Request)/应答(Response)模型。  
#### XMLHttpRequest对象XHR
xhr（ie7及以上浏览器支持，ie更早版本使用ActiveXObject对象）  
##### xhr拥有的方法（客户端）：  
（1）open(method,url,async, bstrUser, bstrPassword)    
    规定请求的类型、URL 以及是否异步处理请求。
    a.)   method：请求的类型，例如：POST、GET、PUT及PROPFIND。大小写不敏感。  
    b.)   url：请求的URL地址，可以为绝对地址也可以为相对地址。  
    c.)   async[可选]：true（默认，异步）或 false（同步）。  
    注释：当您使用async=false 时,JavaScript 会等到服务器响应就绪才继续执行。如果服务器繁忙或缓慢，应用程序会挂起或停止。此时，不需要编写        onreadystatechange回调函数，把代码放到 send() 语句后面即可。  
    d.)   bstrUser[可选]：如果服务器需要验证，此处指定用户名，如果未指定，当服务器需要验证时，会弹出验证窗口。  
    e.)   bstrPassword[可选]：验证信息中的密码部分，如果用户名为空，则此值将被忽略。  
（2）xhr.send(string) 将请求发送到服务器，如果是get类型，参数追在url后面，所以string是null，如果是post请求是携带的数据。  
（3）etRequestHeader(name)  获取指定的相应头部信息  
（4）setRequestHeader(name,value)  自定义HTTP头部信息。需在open()方法之后和send()之前调用，才能成功发送请求头部信息。其中content-type用来定义发送或者接收的实体的MIME类型。默认情况下，服务器对POST请求和提交Web表单不会一视同仁，将Content-Type头部信息设置为application/x-www-form-urlencoded (模拟表单提交)。  
（5）abort()  调用此方法可取消异步请求，调用后，XHR对象停止触发事件，不允许访问任何与响应相关的属性；  
##### xhr拥有的方法（服务端响应）：
（1）onreadystatechange事件，每次readyState状态改变都会触发该事件。  
（2）readyState：存有XMLHttpRequest的状态：  
  0(未初始化)：对象已建立，但是尚未初始化（尚未调用open方法）  
  1(初始化): 对象已建立，尚未调用send方法   
  2: send方法已调用，但是当前的状态及http头未知  
  3：已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误  
  4：数据接收完毕,此时可以通过responseXml和responseText获取完整的回应数据  
（3）返回当前请求的http状态码。  
（4）statusText：返回当前请求的状态文本eg：OK （status：200）  
（5）responseText：将响应信息作为字符串返回  
（6）responseXML：将响应信息格式化为Xml Document对象并返回  
```
var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP');
xhr.open('get', url);
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send(data);
```
