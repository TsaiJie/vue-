#   Promise

### Promise的基本概念

----

1. Promise 是一个构造函数，可以通过 new Promise() 得到一个Promise的实例。

2. Promise 上有两个重要的函数，resolve()***成功之后的回调函数*** 和reject() ***失败之后的回调函数***。

3. Promise 构造函数的 Prototype 属性上有一个 .then()方法。只要是Promise创建的实例，都可以访问到这个方法。

4. Promise 表示一个异步操作；每当我们new一个Promise的实例，这个实例就表示一个具体的异步操作，这个异步操作的结果有两种状态。

   1. 异步执行成功了，需要在内部调用成功的回调函数**resolve**把结果返回给调用者。
   2. 异步执行失败了，需要在内部调用失败的回调函数**reject**把结果返回给调用者。

   由于它是一个异步操作，所以内部拿到的操作结果后，无法使用return把结果返回给调用者，只能使用回调函数的形式，把成功或者失败的结果返回给调用者

5. 可以在Promise的实例上，调用**.then()**方法，预先为这个异步操作，指定成功（**resolve**）和失败（**reject**）的回调函数。

### 形式上和具体上Promise的异步操作

---

```javascript
//形式上的异步操作
//形式上的异步操作的意思是，我们知道它是一个异步操作，但是做什么具体的异步事情，现在尚不清楚
var promise = new Promise();
```



````javascript
//具体的异步操作
const fs = require("fs");
//new实例的时候就会去立即调用function这个异步操作函数
var promise = new Promise(function(){
    //写具体的异步操作
    fs.readFile("./files/2.txt","utf-8",(err, dataStr) => {
        if(err) throw err;
        console.log(dataStr);
    });
});


//有时候我们只是new了实例，不想去立即执行这个函数 ，此时可以这样改造
function getFileByPath(fpath) {
  return new Promise(function (resolve, reject) {
    //写具体的异步操作
    fs.readFile(fpath, "utf-8", (err, dataStr) => {
      if (err) return reject(err);
      resolve(dataStr);
    });
  })
}
//成功的回调函数必须传，失败的回调可以省略
getFileByPath("./files/2.txt")
   .then(function (data){
        console.log(data);
    }, function(err){
        console.g(err.message);
    });

````

### 使用Promise解决回调地狱

---

```javascript

const fs = require('fs')

function getFileByPath(fpath) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fpath, 'utf-8', (err, dataStr) => {
      if (err) return reject(err)
      resolve(dataStr)
    })
  })
}
//成功的回调函数必须传，失败的回调可以省略，
//在上一个.then中，返回一个新的Promise实例，可以继续使用下一个 .then处理
getFileByPath("./files/1.txt")
  .then(function (data) {
    console.log(data);
    return getFileByPath("./files/2.txt")
  })
  .then(function (data) {
    console.log(data);
    return getFileByPath("./files/3.txt")
  })
  .then(function (data) {
    console.log(data);
  });
  console.log("hhhhhh");
```

### Promise捕获异常的两种方式

---

+ 如果，前面的Promise执行失败，我们不想让后续的Promise操作被终止，可以为每个promise指定失败的回调函数，这样就算前面的执行失败，也不影响后面的执行。

  ```javascript
  getFileByPath("./files/11.txt")//文件名错误
    .then(function (data) {
      console.log(data);
      return getFileByPath("./files/2.txt")
    }, function(err){
      console.log("这是失败的回调结果："+ err.message);
      return getFileByPath("./files/2.txt")
    })
    .then(function (data) {
      console.log(data);
      return getFileByPath("./files/3.txt")
    })
    .then(function (data) {
      console.log(data);
    });
    console.log("hhhhhh");
  ```

+ 如果后续的Promise执行依赖于前面Promise执行的结果，如果前面的执行失败，后面就不执行了。一旦报错就立即终止所有的Promise的执行。

  ```javascript
  getFileByPath("./files/1.txt")
    .then(function (data) {
      console.log(data);
      return getFileByPath("./files/22.txt")//文件名错误
    })
    .then(function (data) {
      console.log(data);
      return getFileByPath("./files/3.txt")
    })
    .then(function (data) {
      console.log(data);
    })
    .catch(function(err){//catch的作用：如果前面有任何的Promise执行失败，
      //则立即终止所有promise的执行，并马上进入catch去处理promise中抛出的异常
      console.log("这是自己的处理方式：" + err.message);
    })
    ;
    console.log("hhhhhh");
  ```

  

