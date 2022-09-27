---
title: 在vue项目里实现百度地图坐标转换
categories:
- tech
---

# 在vue项目里实现百度地图坐标转换

> 背景

​		背景是这样的，因项目需要，在网页中要放置一张百度地图，用来显示各种不同局站的地理分布，大概有几百个坐标需要同时显示。而甲方给的数据表中的坐标是大地坐标系（WGS84），而百度地图使用的则是自己的百度坐标系（bd09），因此直接显示会有偏移而不准确，所以要进行纠偏后才能正常显示。

> 百度提供的接口

百度坐标转换接口的服务文档：
https://lbsyun.baidu.com/index.php?title=webapi/guide/changeposition

接口：
http://api.map.baidu.com/geoconv/v1/?coords=114.21892734521,29.575429778924&from=1&to=5&ak=你的密钥 //GET请求

按照服务文档的说明，可以通过这个接口提交坐标转换的申请，即发送需要转换的坐标、源坐标类型、目标坐标类型，从而得到一个状态码和坐标转换后的结果，其格式如下：

```js
{"status":0,"result":[{"x":117.19322369775665,"y":39.12574664331678}]}
```

从这个返回值中可以取回坐标值应用到我们的地图组件中。

> 原始数据

在项目里我们通过`axios`从后台获取到数据表并放入**pointData**数组中

```js
axios({
      method: "get",
      url: "...",
    }).then((res) => {
      if (res.data.success) {
        this.pointData = res.data.data;
      };
    })
```

**pointData**包含了多个数组对象，每个数组对应不同类型的局站，每个数组下又包含100个左右具体局站的数据信息。

例如某一个局站的经度可以这样调用

```js
pointData[0][0].lat //该局站的维度
```

> 面临问题

**问题（一）：**跨域

​		在本地如果直接用`axios`调用百度接口会因为浏览器的跨域资源共享（CORS）机制而导致无法获取数据

**问题（二）：**发送坐标

​		百度地图接口单次最多可以接收100个坐标，因此需要将原始数据里的坐标提出来并拼接成为一个字符串

**问题（三）：**异步执行问题

​		每一次向接口发送请求都是一个异步操作，而我们数据的组装是有顺序的，需要进行同步操作才能够实现我们的功能

> 解决方案

**一：**使用JSONP解决跨域问题

​		Jsonp（JSON with Padding）是 json 的一种"使用模式"，可以让网页从别的域名（网站）那获取资料，即跨域读取数据。

---

1. 首先安装jsonp依赖

   ```js
   npm i vue-jsonp --save
   ```

2. 接着在main.js上导入并安装

   ```js
   import VueJsonp from 'vue-jsonp'
   Vue.use(VueJsonp)
   ```

3. 接下来就可以通过调用`this.$jsonp(url)`来进行跨域访问链接了

   ```js
   this.$jsonp(url).then((res)=>{
   	if (!res.status) {
       	//...处理语句
           }})
   ```

**二：**进行字符串拼接（坐标转换方法）

​		可以将数据中的坐标提取出来每100个拼接成为一个字符串并进行一次转换申请，如果对每一个坐标单独转换的话，会产生相当高的并发，容易超过百度接口的额度，并且会造成服务器的延迟，增加复杂度。

---

```js
async converted(){
    //s为拼接后的字符串
    var s = "";
    var i = 0;
    //points为装载所有转换后的点的坐标列表
    var points = [];
    //temp为临时存放坐标的集合
    var temp = [];
	//字符串拼接并发送请求
    for(var j = 0;j<this.pointData.length;j++){
        // console.log(`循环第${j}次`)
        for(let item of this.pointData[j]){
            s += item.lng + "," + item.lat + ";"
            i++;
            //到100个时开始发送请求并将返回值保存
            if(i == 100){
                //发送一次请求并等待返回
                let res = await this.send(s);
                //将这次返回结果装入temp数组
                temp = temp.concat(res.result);
                //清空坐标进行下一轮请求
                s = "";
                i = 0;
            }
        }
        //将剩下的坐标发送请求
        let res = await this.send(s);
        //返回的坐标全部保存在temp数组中
        temp = temp.concat(res.result);
        //将这一组坐标装入points数组中
        points.push(temp);
        temp = [];
        s = ""; 
    }
    //对原始坐标进行转换
    for(var j = 0; j < this.pointData.length; j++){
        //console.log(`第${j}组转换`)
        for (let [index,item] of this.pointData[j].entries()) {
            item.lng = points[j][index].x;
            item.lat = points[j][index].y;
        }
    }
    return 0;
}
```

**三：**使用**async**、**await**关键字和**promise**对象解决异步问题

> > Event Loop

​		JavaScript语言一种单线程语言，所有任务都在一个线程上完成，单线程的运行方式为**“同步模式”（synchronous I/O）**或**“堵塞模式”（blocking I/O）**

​		于是就采用**Event Loop**机制，来解决单线程运行带来的一些问题。就是在程序中设置两个线程：一个负责程序本身的运行，称为**"主线程"**；另一个负责主线程与其他进程（主要是各种I/O操作）的通信，被称为**"Event Loop线程"（可以译为"消息线程"）**。

​		每当遇到I/O的时候，就**Event Loop**线程去做，而主线程继续往后运行。当I/O操作完成，**Event Loop**线程再把结果返回，完成调用其回调函数完成任务。

​		于是，主线程可以在相同时间运行更多的任务，这种运行方式称为**“异步模式”（asynchronous I/O）**或**“非堵塞模式”（non-blocking mode）**。

---

​		正是由于**Event Loop**机制的存在，程序中的I/O操作都是异步模式。在前面的坐标转换方法中，我们的`pointData`数组中装了很多组坐标，每一组都是某一种类型局站的坐标，因此整个转换请求方法需要两个for循环才能全部转换完成。

​		当我们仅实现一组的坐标转换时，**Event Loop**机制并不会对结果产生影响，而进行下一组转换前，`temp`的值要`push`到`points`中之后并被清空，此时问题就来了。

​		我们知道，**Event Loop**线程会和主线程分离，**也就是说for循环并不管我们发送的I/O请求是否返回了结果，就继续往下执行**。有可能temp还是空的时候，我们的主线程的一轮循环就结束了，结果这一组坐标就没有被装进去，可能被装进下一组中甚至丢失，总之无法实现我们想要的结果。

​		也就是说，我们希望这个程序是**完全同步**的，每一个语句完全执行完毕再进行下一个语句，但是**Event Loop**导致他是异步的，所以便出现了错误。

要想解决这个问题，我们就需要使用**async**、**await**关键字和**promise**对象了。

---

1. **async、await**关键字

   **async**用于声明一个函数是异步的。而**await**从字面意思上是“等待”的意思，是`async await`的简写，用于等待异步完成。并且**await**只能在**async**函数内使用，**简单理解就是，async 声明的函数内的异步会按照同步执行顺序**。通常**async**、**await**都是跟随**Promise**一起使用的。

   在希望其内运行顺序为同步的函数名前加上`async`关键字，在其中的异步I/O操作前加上`await`关键字

   ```js
   let res = await this.send(s);
   ```

   我们的`s`是一个坐标字符串，即待转换的一组坐标，`send()`函数是一个I/O请求，`res`为返回的转换后的坐标，加上`await`关键字之后，程序运行到这一句将等待`send()`函数返回，即res中得到返回值后再向下运行

2. **promise**对象

   **promise**,简单来说就是一个容器，里面保存着某个未来才会结束的时间(通常是一个异步操作的结果)

   **promise**有三个状态：**pending**（执行中）、**success**（成功）、**rejected**（失败），当执行成功会返回`resolve()`中的值，失败则会返回`reject()`中的值

   **await**后面要跟一个**promise**对象才能进行阻塞，若不是**promise**对象，则还是会直接执行下面的语句。

   ```js
   send(s){
         return new Promise((resolve, reject)=>{
           s = s.substring(0,s.length-1);
           s = "http://api.map.baidu.com/geoconv/v1/?coords=" + s + "&from=3&to=5&ak=DZSjokGwq1OaHmR4iEhN4O0TXb0ImGBS";
           this.$jsonp(
             s
           ).then((res)=>{
             if (!res.status) {
               resolve(res)
           }
           }).catch(err =>{
             reject(err)
           });
         })
       }
   ```