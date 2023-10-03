# async & await

- **await** 关键字作用：

  > 等待后面的 **promise** 对象的 **resolve** 状态，若后面为非 __promise__ 对象，会将其用`Promise.resolve() `转换。  
  >
  > 监听到 **resolve** 状态之后，将 ***await下面的语句*** 推送到 _微队列_；若无语句，则将 ***async 函数结束操作***   推送到 _微队列_

___

## 例题

```js
async function asy1(){
    
    console.log(1)

    await asy2()

    console.log(2)

}

async function asy2(){

    await setTimeout(()=>{
        Promise.resolve().then(()=>{
            console.log(3)
        })
        console.log(4)
    },0)

}

async function asy3() {
    Promise.resolve().then(()=>{
        console.log(6)
    })
}

asy1()

console.log(7)

asy3()

//输出结果 1,7,6,2,4,3
```

---

- 如何判断一个函数是否是 ***async*** 函数？ 

  > 解决办法：
  >
  > async函数的原型是 __AsyncFunction__
  >
  > async函数中有个Symbol.toStringTag，`func[Symbol.toStringTag]='AsyncFunction'`

---



# JS基本数据类型

* ## 基本数据类型

  > String、Number、Boolean、Undefined、Null

* ## 引用数据类型

  > Object(Array、Date、RegExp、Function)
  
---



# 拷贝

* ## 浅拷贝
	> 浅拷贝会将对象的每个属性进行依次复制，但当属性是一个引用类型时，实际复制的是一个其引用，当指向的引用发生改变时，也会随之变化。
* ## 深拷贝
	> 深拷贝复制变量的值，对于引用数据类型，则递归至基本类型，再进行复制。
	>
	>   
	>
	> 深拷贝的对象与原对象是完全隔离的，互不影响。

* ## 区别

	> 在有指针的情况下，浅拷贝只是增加了一个新的指针指向一个已存在的内存，而深拷贝则是增加一个新指针，并且申请一个新的内存，这个新指针指向新内存。

---



# 相等

 * ### 宽松相等 ==

   > 宽松相等会在进行比较之前将两边操作数进行**强制类型转换**，故只要操作数值相同，就返回**true**

 * ### 严格相等 ===
   
   > 严格相等**不进行强制类型转换**

---



# 模块化标准

* ### CommonJS 社区标准

  > **CommonJS** 是2009年，由JavaScript社区提出的一个模块化标准，后被 **Node.js** 实现。

  导出方式有**三种**：
  
  1. **module.exports对象**
	
	```js
	module.exports={
	    变量1,
      变量2
  }
  ```
	
	2. **exports对象**
  
	```js
	exports.var1=var1
	exports.var2=var2
	```
	
	3. **this对象**
	
	```js
	this.var1=var1
	this.var2=var2
	```
	
	> 这三个对象，初始时都指向一个 **module.exports={} ** 空对象
  
  导入方式：
	
	```js
	const impo=require("/test.js")
	```
	
	> 导入函数 ```require()``` 最终返回的是 ```mudole.exports``` 对象
	
	
	
	**注意！**CommonJS中的导入是对原始值的**拷贝**，改变被导入模块中的值不会影响当前模块的值。而 **ESM** 中，导入是对原始值的**引用**。
	
	
	
* ### ES Module 官方标准

  >  **ES Module** 是ES6时提出的官方标准。
  
  导出
  
  1. 命名导出
  2. 默认导出