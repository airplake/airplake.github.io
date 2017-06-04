---
title: 不得不学的AJAX
date: 2017-06-4 10:45:38
categories: 前端开发
tags:
  - AJAX
---





## ajax简介

***

###  一、为什么要学ajax?

* 从个人工作待遇来说,如果你想从 *4k*  ->  **8k**，**ajax** 就是你必须要学习的技术。现在我们来看下各大网站招聘要求


![zp02.png](http://upload-images.jianshu.io/upload_images/1430231-e3bb00ad70b7c45e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

![zp03.png](http://upload-images.jianshu.io/upload_images/1430231-8c88d9aede493637.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


从各大招聘网站的要求可以看出，只要你是前端工程师，就必须掌握**ajax**，想不想成为一名优秀的前端工程师？想不想涨工资？想不想学？



* 网站使用率

90%网站基本都使用**ajax**，包括腾讯、阿里、360、百度等等，没有使用的基本是以前的老网站，没有升级和进行维护。








### 二、ajax是什么

#### 2.1 什么是**ajax**
>AJAX即“Asynchronous Javascript And XML”（ **异步** JavaScript和XML），AJAX不是一种新的编程语言，而是一种用于创建**更好更快以及交互性更强的** Web 应用程序的技术。它是一套综合了多项技术的浏览器端网页开发技术。这些技术包括Javascript、XHTML和CSS、DOM、XML和**XMLHttpRequest**.

通过在浏览器与服务器进行**少量数据**交换，AJAX 可以使网页实现**异步更新**。这意味着可以在不重新加载整个网页的情况下，对网页的**局部更新**。


#### 2.2 ajax优点&&缺点

* 优点:

  使用Ajax的最大优点，就是能在不更新整个页面的前提下维护数据。这使得Web应用程序更为迅捷地响应用户动作，并避免了在网络上发送那些没有改变的html代码信息。


  1. 减轻服务器负担，按需要获得数据。
  2. 无刷新更新页面，减少用户的实际和心理的等待时间。
  3. 更好的用户体验。
  4. 减轻宽带的负担。
  5. 主流浏览器支持

*  缺点:

  AJAX不是完美的技术。也存在缺陷：

1. AJAX大量使用了Javascript和AJAX引擎，而这个取决于浏览器的支持。IE5.0及以上、Mozilla1.0、NetScape7及以上版本才支持，Mozilla虽然也支持AJAX，但是提供XMLHttpRequest的方式不一样。所以，使用AJAX的程序必须测试针对各个浏览器的兼容性。
2. AJAX更新页面内容的时候并没有刷新整个页面，因此，网页的后退功能是失效的；有的用户还经常搞不清楚现在的数据是旧的还是已经更新过的。这个就需要在明显位置提醒用户“数据已更新”。
3. 对搜索引擎支持不好。  


#### 2.4 哪里用到Ajax

现在网站几乎无不使用Ajax.具有代表使用Ajax技术的网站有:新浪微博、Google 地图、Gmail、开心网等等.其中Gmail(google的邮箱)是开辟了Ajax技术的先河.

举例: [腾讯登陆](http://www.qq.com/)、[百度](http://www.baidu.com/)、[阿里云](https://account.aliyun.com/find_loginid/findLoginId.htm)

#### 2.2 ajax原理

>Ajax的原理简单来说通过浏览器的XmlHttpRequest(Ajax引擎)对象来向服务器发异步请求并接收服务器的响应数据，然后用javascript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。即用户的请求间接通过Ajax引擎发出而不是通过浏览器直接发出,同时Ajax引擎也接收服务器返回响应的数据,所以不会导致浏览器上的页面全部刷新.


![2_ajax.png](http://upload-images.jianshu.io/upload_images/1430231-31e3d2c74bd828e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

举例：

传统方式的注册表单的验证:


![4_传统请求.png](http://upload-images.jianshu.io/upload_images/1430231-ca33fe1730451dae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ajax方式的注册表单的验证:


![5_Ajax请求.png](http://upload-images.jianshu.io/upload_images/1430231-76865557eaa44c87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**注意：真正的请求是由Ajax引擎发出.不是由浏览器窗口发出,所以浏览器窗口是不会刷新的, Ajax引擎同时也接收服务器返回的响应内容.**


<--end-->

#### 2.3 Ajax引擎(XMLHttpRequest)

##### 2.3.1 什么是Ajax引擎

Ajax引擎就是XMLHttpRequest对象,所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。它同是也是一个Javascript对象.


##### 2.3.2.为什么使用Ajax引擎

Ajax引擎(XMLHttpRequest)是Ajax综合技术的核心,其作为浏览器页面和服务器交互的一个桥梁.


![ajax04.png](http://upload-images.jianshu.io/upload_images/1430231-106cf273bb34d539.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通常是Javascript监听浏览器网页事件(点击,提交,更改等),由Javascript创建Ajax引擎对象,通过Ajax引擎对象发出请求.Ajax引擎等待并且接收服务器的响应内容,Javascript再从Ajax引擎对象中获取响应内容并且改变网页界面显示效果.

##### 2.3.3 如何创建Ajax引擎对象

1.非IE浏览器和高版本的IE浏览器

    var xhr = new XMLHttpRequest();

2.老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象.

    var xhr = new ActiveXObject("Microsoft.XMLHTTP");


兼容处理

    //Ajax.js    
    function createAjax() {
      var xhr ;
      if(window.XMLHttpRequest){
        //所有现代浏览器 (IE7+、Firefox、Chrome、Safari 以及 Opera) 都内建了 XMLHttpRequest 对象。
        xhr = new XMLHttpRequest();
      }else {
        //老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：
        xhr = new ActiveXObject("Microsoft.XMLHTTP");
      }

      return xhr;
    }






#### 2.4 如何使用Ajax引擎对象

##### 2.4.1 基本语法

使用Ajax引擎对象就是使用它上面的方法和属性完成发送请求和接收响应内容等.

注意:Ajax请求和响应也符合Http协议.

**方法**

|方法名|注释|
|-----|----|
|abort|取消当前请求|
|getAllResponseHeaders|获取响应的所有http头|
|getResponseHeader|从响应信息中获取指定的http头|
|open(方式get/post，url地址，同步异步|创建一个新的http请求，打开请求，并指定此请求的方法、URL以及验证信息(用户名/密码)|
|send()|发送请求到http服务器并接收回应|
|setRequestHeader|单独指定请求的某个http头，header()设置http头协议信息|


**属性**

|属性名|注释|
|-----|----|
|onreadystatechange|指定当readyState属性改变时的事件处理句柄。|
|readyState |返回当前请求的状态，ajax行进过程中不同状态|
|responseBody |将回应信息正文以unsigned byte数组形式返回.|
|responseStream|以Ado Stream对象的形式返回响应信息。|
|responseText|将响应信息作为字符串返回.只读|
|responseXML|将响应信息格式化为Xml Document对象并返回，只读|
|status|返回当前请求的http状态码. 200 ok   304 缓存 ；  404  not found;  403 没有权限  501 服务器级别错误|
|statusText|返回当前请求的响应行状态文本，  ok   not found    forbidden|


基本使用:

    //>>1.创建Ajax引擎对象
    var xmlHttpRequest= createAjax();
    //>>2.设置发送请求时需要具备的数据
      //>>2.1.指定请求url地址(注意还没有发出请求仅仅是设置请求地址和请求方式)
      xmlHttpRequest.open('GET/POST','url地址',[ '是否异步']);
      //>>2.2.监听Ajax引擎对象的改变
      xmlHttpRequest.onreadystatechange=function(){
          //当请求和响应同时完成,从Ajax引擎中获取响应内容,然后通过javascript对网页进行操作
          if (xmlHttpRequest.readyState==4 && xmlHttpRequest.status==200){
              //根据相应内容对网页进行操作的代码通常写在这里
              // xmlHttpRequest. responseText
    //          //xmlHttpRequest. responseXML
          }
      }
    //>>3.发送请求
    xmlHttpRequest.send([post请求参数字符串]);

##### 2.4.2 ajax get 注册例子

##### 2.4.3 ajax post 注册例子

##### 2.4.4 详解onreadystatechange

onreadystatechange属性是一个方法,当Ajax引擎的状态发生改变时都会执行onreadystatechange属性对应的方法.

**ajax引擎的状态属性readyState（0  1  2  3  4）**

* 0.请求未初始化
* 1.服务器连接已建立
* 2.请求已接收
* 3.请求处理中
* 4.请求已完成，且响应已就绪

见图：


![readyState.png](http://upload-images.jianshu.io/upload_images/1430231-9c2a3cc52a4c3689.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


当前ajax引擎的状态属性**readyState为4**时,说明服务器的响应已经发送给ajax请求了. 但是响应的状态码为200时说明响应的内容是正确的,这时才会根据相应内容对当前页面的html处理.




#### 2.6 同步

### 三.强调(重点)

ajax请求是由html页面通过javascript发出的.
发送请求分为两种情况:

1. 监控用户的事件(onclick,onchange等),通过ajax发送请求
2. 监控浏览器的事件(onload),通过ajax发送请求.
onreadystatechanage方法监控到响应内容的返回, 根据响应内容使用javascript改变当前页面的部分html代码,从而达到不刷新改变局部html代码.
