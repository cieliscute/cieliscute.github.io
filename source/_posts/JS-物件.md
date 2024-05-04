title: '[JS] 物件基本介紹'
author: Wei
tags: []
categories:
  - Tmp
date: 2024-05-04 02:15:00
---
## 創建物件的方法
- 物件實字 `{}`
- 建構函式 `new Object`

ps:建構函式產生的包裹物件可能會因為不同內容而使用不同的建構函式  
e.g: new Object(1) >> Number({1}) 
e.g: new Object('1') >> String({"1:"})
e.g: new Object({myName:'小明'}) >> Object { myName: "小明" }

**所以建議還是使用物件實字的方式建立物件就好**

## 物件取值
物件取值有兩種方式，先使用**物件實字**方式建立物件如下:
```js
const family={
	name:'小明家',
    deposit:1000,
    members:{
    	mom:'老媽',
        ming:'小明',
    },
    callFamily:function(){
    	console.log('聯絡小明家')
    },
    1:'只能用中括號取得此值',
    '2':'屬性一律為字串',
    '$#@!':'$#@!-string'
}
```

取值的方式主要有兩種，分別為:
1. 點記法
2. 中括號取值

### 點記法
以上方物件範例來說，我們需要取得小明家存款(deposit)的內容，可以使用`family.deposit`取得。如果要調用`callFamily`這個函式的話，則是使用`family.callFamily()`即可執行該函式。

點記法雖然很方便，但是他無法讀取數字或特殊符號的屬性，這時候就要使用中括號取值來解決。


### 中括號取值
中括號取值需要注意的地方在於，中括號內需要填入的是字串。

例如同樣要取得存款(deposit)的值時，我們需要改寫為`family['deposit']`，中括號必須填入字串。

同樣的，要取得`1:'只能用中括號取得此值'`的內容，需要使用`family['1']`來取得。但是這邊使用`family[1]`也可以，因為會自動將數值1轉型為字串1。

但是如果需要取得`'$#@!':'$#@!-string'`的內容時，就必須要輸入`family['$#@!']`。

總結來說，因為物件的屬性必定是字串形式，所以在用中括號取值時一律使用字串就不會有問題了。

補充:
使用中括號取值時，也可以用變數的形式傳入字串。例如:
```js
const foo='$#@!'
console.log(family[foo]) // "$#@!-string" 
```
透過這種寫法可以增加程式碼撰寫時的彈性


## 新增屬性
一樣可以透過**點記法**或是**中括號**兩種方式增加
```js
family.getMembers=function(){return this.members}
family['members']['dad']='老爸'

family.getMembers();  // Object { mom: "老媽", ming: "小明", dad: "老爸" }
```

## 刪除屬性
- [屬性的刪除](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/delete) | MDN

可以透過`delete`運算子 來刪除物件的屬性:

```js
delete family.deposit //true
```

如果刪除成功的時候會回傳true，但是就算原本該物件就沒有被指定刪除的屬性，仍然會return true(e.g:delete family.ooxx >> true)

那麼什麼時候會回傳false呢? 如果操作無效的話就會回傳false。例如想要直接刪除family本身，但是因為family並不是一個物件的屬性，所以會回傳false，並且family也不會被刪除。

```js
delete family //false
console.log(family) // {name:'小明家',....}
```

需要特別注意的地方是，當我們不使用`var`,`const`,`let`宣告的時候，實際上是在`window`底下新增該屬性，例如:
```js
var foo='var宣告的foo'
function addBarUnderWindow(){
bar='新增在全域window底下的bar屬性'
}
addBarUnderWindow(); 
console.log(window)  //這邊可以看到window下面多了一個bar的屬性
delete foo // false
delete bar // true
```


## ESLint風格補充
有些撰寫風格會希望在物件內最後一個屬性後方再加上逗號，但是不加也不會有任何影響。

## 未定義之屬性
如果我們今天要存取沒有定義的屬性，會回傳`undefined`。假設我們又在該屬性下要新增物件屬性，會產生錯誤:
```js
const family={
    name:'小明家',
    deposit:1000,
}
console.log(family.ooxx.foo) // Uncaught TypeError: family.ooxx is undefined
console.log('我沒有辦法被運行，因為前面出現錯誤了')
```

當出現錯誤時，後方的javascript程式碼都不會被運行。

要解決這個問題可以預先先定義好`ooxx`是一個空物件:
```js
const family={
    name:'小明家',
    deposit:1000,
    ooxx:{}
}
family.ooxx.foo='undefined下的屬性'
```

或是可以用可選串聯(`?.`)運算子來解決

- [可選串聯](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Optional_chaining) | MDN

```js
const family={
    name:'小明家',
    deposit:1000,
}
console.log(family.ooxx?.foo) //undefined
console.log('我可以被運行')
```
可以發現只沒有出現錯誤，而是出現undefined。

另外，如果在取得全域(window)下屬性出現錯誤時，造成後方程式碼無法運行的狀況，也可以透過對window物件存取屬性來解決這個問題:

```js
console.log(a) // 拋出錯誤
console.log('我不會被執行，因為a沒有定義，拋出錯誤');
```

```js
console.log(window.a) //undefined
console.log('可以被執行');
```






