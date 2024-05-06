title: '[JS] prototype原型'
author: Wei
tags: []
categories:
  - Tmp
date: 2024-05-06 00:33:00
---
## 參考資料
- [物件導向基本觀念](https://javascript.alphacamp.co/basic-idea-of-oo.html)

## 建構函式
Javascript原本就提供一系列建構函式，例如`Object`、`String`、`Boolean`、`Date`,`Promise`...etc。

**PS:一般來說我們習慣用大寫來表示建構函式**

### 建立Dog原型
建構函示其實與一般函示沒有差別，都是使用`function`來宣告，我們來簡單創建一個建構函式：
```js
function Dog(name,size,color){
	this.name=name
    this.size=size
    this.color=color
}
```
接著我們使用`new`運算子來透過這個`Dog`建構函示建立實體(instance)
```js
const white=new Dog('小白','小型犬','白色')

console.log(white) //Dog {name: '小白', size: '小型犬', color: '白色'}
```

使用`new`運算子時，會進行以下動作：
1. 創建一個空物件實體`{}`
2. 接著將該函式的`this`指向步驟一創建的空物件
3. 依序執行(e.g:this.name=name會在物件內創建一個name屬性，並且將參數name的值賦予到該屬性上)


經過以上動作，我們建立了一個建構函示`Dog`的原型，並且利用`new`運算子生成一個名叫小白的小型犬實體。

## 新增新的Array方法
使用原型的好處是，我們可以直接新增方法在原型上，而不需要每次都重新定義。在修改方法時也只要修改一個地方，就可以在所有的實例上調用修改後的方法。

我們都知道可以直接透過`const ary1=[1,2,3,4]`來建立一個陣列，接著我們要試著新增一個**取出陣列最後一個元素**的方法：

```js
const ary1=[1,2,3,4,5]

Array.prototype.getLastElement=function(){
	return this[this.length-1]
}

console.log(ary1.getLastElement()) //5
```

**這邊需要注意，在定義原型方法時，需要使用函式表達式來定義，不要使用箭頭函式，否則`this`的指向可能會有錯誤**
```js
//錯誤範例
const ary2=[6,7,8,9]
Array.prototype.getFirstElement=()=>{
	return this[0];
}
console.log(ary2.getFirstElemnt() //expect 6, but undefined
console.log(ary2.getLastElement() //9
```
因為在定義`getFirstElement`時使用箭頭函式，this指向window，所以this[0]會是undefined。

並且我們也同樣可以在ary2上使用先前所定義的`getLastElement`方法


## 新增Dog

## [[prototype]]
先前我們有建立一個狗原型的實例`white`，當我們透過`console.log(white)`展開物件時，可以看到一個`[[prototype]]`顯示他是**Dog原型**，並且裡面存在一個`constructor:function Dog(name,size,color)`，指向建構函示`Dog`本身

p.s:有些瀏覽器可能會使用`__proto__`來表示`[[prototype]]`，並且在自己建立的原型產生的實例下，該實例的`[[prototype]]`在Safari會顯示`Dog`,但Firefox,Chrome,Arc等瀏覽器都是顯示為`Object`。這邊我認為`Dog`會比較恰當。

另外需要注意，這邊的`[[prototype]]`與我們前面在撰寫的Array.**prototype**這裡的prototype是不一樣的東西！


先前我們有在Array原型上新增我們自己的方法，現在我們嘗試在



(雖然在ES6之後有提供`class`搭配`constructor`來創建建構函式，但是這個寫法只是語法糖，實際上仍然是原型鏈)