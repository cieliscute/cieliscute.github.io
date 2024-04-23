title: '[JS] 原型'
author: Wei Wei
tags:
  - Javascript
categories:
  - Tmp
date: 2024-04-23 01:31:00
---
## 參考資料
- [Javascript ES6 Class 與建構函式 ](https://clairechang.tw/2023/03/06/javascript/js-class/)  
- [alphacamp 物件導向基本觀念 ](https://javascript.alphacamp.co/basic-idea-of-oo.html)
- [卡斯柏鐵人賽：JavaScript 的原型繼承](https://www.casper.tw/javascript/2017/12/17/javascript-prototype/)

---
<!-- more -->

在實作物件導向時,由於缺少類別的概念,JavaScript 通常採用以下模式:

1. 建構函數搭配原型(Constructors + Prototypes)
2. 物件字面量(Object Literals)
3. 工廠函數(Factory Functions)
4. 使用ES6的類別語法糖(Class Syntax)

範例如下

建構函數搭配原型(Constructors + Prototypes) 這種模式使用函數來模擬類別的建構子,通過new關鍵字產生新的實例對象,同時利用原型對象共享方法和屬性。
```javascript
// 建構函數
function Person(name) {
  this.name = name;
}

// 在原型上定義方法
Person.prototype.sayHello = function() {
  console.log('Hello, my name is ' + this.name);
}

// 創建實例
let john = new Person('John');
john.sayHello(); // 輸出 "Hello, my name is John"
```

物件字面量(Object Literals) 這是最簡單的一種模式,通過簡單的對象字面量來創建對象,適用於簡單的情況。
```javascript
// 定義對象字面量
let person = {
  name: 'John',
  sayHello: function() {
    console.log('Hello, my name is ' + this.name);
  }
}

person.sayHello(); // 輸出 "Hello, my name is John"
```

工廠函數(Factory Functions) 工廠函數是一種返回新對象的函數,可實現對象的創建和初始化
```javascript
// 工廠函數
function createPerson(name) {
  let obj = {};
  obj.name = name;
  obj.sayHello = function() {
    console.log('Hello, my name is ' + this.name);
  }
  return obj;
}

// 創建對象
let john = createPerson('John');
john.sayHello(); // 輸出 "Hello, my name is John"
```

使用ES6的類別語法糖(Class Syntax) ES6新增了類別語法糖,雖然本質上還是基於原型繼承,但讓代碼看起來更像傳統的類別繼承模式。

```javascript
// 定義類別
class Person {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

// 創建實例
let john = new Person('John');
john.sayHello(); // 輸出 "Hello, my name is John"
```


---
```
// 工廠函數

function createPerson(name) {

let obj = {};

obj.name = name;

obj.sayHello = function() {

console.log('Hello, my name is ' + this.name);

}

return obj;

}

// 創建對象

let john = createPerson('John');

john.sayHello(); // 輸出 "Hello, my name is John"
```

這段程式碼定義了createPerson的function，但是這個function內的物件所定義的sayHello function，會在每次呼叫createPerson function的時候都產生。這樣每次都會佔據記憶體來記憶重複的function。請問這部分是否可以在createPerson內另外定義一個建構函式，並且在該函式的prototoype加上sayHello的方法，最後使用new運算子產生一個instance並回傳，這樣是否可以有效減少程式碼的記憶體消耗，並且在之後維護的時候也只要修改該建構函式的sayHello方法就可以一次性修改所有的instance?

	是的,您提出的優化方式非常好,可以有效減少記憶體消耗並提高維護效率。我們可以定義一個建構函數Person,將sayHello方法放在它的原型prototype上,然後使用new運算子創建實例對象並返回。這樣所有實例對象就共享同一個sayHello方法,大大節省了記憶體。同時,如果需要修改sayHello方法,只需修改原型上的方法,所有實例對象都會受到影響。優化後的代碼如下:
	

```javascript
// 定義建構函數
function Person(name) {
  this.name = name;
}

// 在原型上定義sayHello方法
Person.prototype.sayHello = function() {
  console.log('Hello, my name is ' + this.name);
}

// 工廠函數
function createPerson(name) {
  // 使用new運算子創建實例對象
  return new Person(name);
}

// 創建對象
let john = createPerson('John');
john.sayHello(); // 輸出 "Hello, my name is John"
```

在這個優化後的版本中:

我們定義了一個Person建構函數,用於初始化name屬性。
將sayHello方法定義在Person.prototype上,這樣所有由Person建構函數創建的實例對象都共享這個方法。
在createPerson工廠函數中,使用new運算子創建Person實例對象,並將其返回。

使用這種方式,無論創建多少個Person實例對象,它們都共享同一個sayHello方法,大大減少了記憶體消耗。而且,如果需要修改sayHello方法,只需要修改Person.prototype.sayHello即可,所有實例對象都會自動獲得修改後的版本,無需逐個修改,提高了維護效率。



---

請問如果我把建構函式放置在function createPerson內，是否會影響程式碼的運作?


	如果將建構函數放置在 createPerson 函數內部,程式碼仍然可以正常運作,但會有一些效能和設計上的影響:
    
```
    function createPerson(name) {
  	// 建構函數定義在內部
	  function Person(name) {
    this.name = name;
	  }

 	 // 在原型上定義 sayHello 方法
 	 Person.prototype.sayHello = function() {
 	   console.log('Hello, my name is ' + this.name);
	  }

	  // 使用 new 運算子創建實例對象
	  return new Person(name);
	}

	// 創建對象
	let john = createPerson('John');
	john.sayHello(); // 輸出 "Hello, my name is John"
```

這種做法的影響如下:

1. 效能影響：每次呼叫 createPerson 函數時,都會重新定義一次 Person 建構函數和 sayHello 方法,造成不必要的重複工作,影響效能。
2. 記憶體消耗：由於每次呼叫 createPerson 時都會創建新的 Person 建構函數實例,導致多佔用一些記憶體空間。
3. 設計問題：將建構函數封裝在工廠函數內部,雖能隱藏建構函數的實現細節,但降低了建構函數的可重用性和維護性。如果需要在其他地方使用 Person 建構函數,就必須重複定義或從工廠函數中抽離出來。

總的來說,將建構函數放在工廠函數內部是可行的,但不太符合物件導向設計的最佳實踐。更好的做法是將建構函數單獨定義在全域作用域或模組作用域中,以提高可重用性、效能和維護性。工廠函數只負責使用建構函數創建對象實例並返回即可。

---

我知道建構函式可以有一些靜態方法，讓該方法只能給建構函式本身所使用。使用建構函式生成的instance並不會繼承該方法，這個是否稱為靜態方法?

另外，請以上述的Person建構函式，來撰寫一個靜態方法的範例給我

	是的,您說的完全正確。在建構函數中定義的方法,如果使用 static 關鍵字,那麼這個方法就是一個靜態方法。靜態方法只能由建構函數本身調用,而不能被實例對象調用。實例對象無法繼承靜態方法。

	以上面的 Person 建構函數為例,我們可以這樣添加一個靜態方法:
    
person.js

```javascript
// 定義建構函數
function Person(name) {
  this.name = name;
}

// 在原型上定義sayHello方法
Person.prototype.sayHello = function() {
  console.log('Hello, my name is ' + this.name);
}

// 定義靜態方法
Person.createAnonymous = function() {
  return new Person('Anonymous');
}

// 導出建構函數
export default Person;
```

在這個例子中,我們為 Person 建構函數添加了一個靜態方法 createAnonymous()。這個方法可以用來快速創建一個匿名的 Person 實例對象。

我們可以這樣使用這個靜態方法:

main.js
```javascript
// 導入Person建構函數
import Person from './person.js';

// 使用靜態方法創建匿名實例
let anonymous = Person.createAnonymous();
anonymous.sayHello(); // 輸出 "Hello, my name is Anonymous"

// 嘗試從實例調用靜態方法
anonymous.createAnonymous(); // 錯誤: anonymous.createAnonymous is not a function

```

從上面的例子可以看出:

我們可以通過 Person.createAnonymous() 直接調用這個靜態方法,並獲得一個匿名的 Person 實例對象。

但是如果嘗試從實例對象 anonymous 上調用 createAnonymous() 方法,就會報錯,因為實例對象無法繼承靜態方法。

靜態方法的典型應用場景包括:

    創建實用程序函數,例如創建對象的工廠方法
    操作類別本身的數據或屬性
    實現一些單例模式等設計模式

總的來說,靜態方法為建構函數添加了一些與實例無關的功能方法,增強了建構函數的功能,同時也遵循了類別設計的最佳實踐。

---

轉為語法糖

好的,我將使用 ES6 的類別語法糖(Class Syntax)來重寫上面的代碼:

person.js
```javascript
// 定義 Person 類別
class Person {
  // 建構函數
  constructor(name) {
    this.name = name;
  }

  // 實例方法
  sayHello() {
    console.log(`Hello, my name is ${this.name}`);
  }

  // 靜態方法
  static createAnonymous() {
    return new Person('Anonymous');
  }
}

// 導出 Person 類別
export default Person;
```
main.js
```javascript
// 導入 Person 類別
import Person from './person.js';

// 使用建構函數創建實例
let john = new Person('John');
john.sayHello(); // 輸出 "Hello, my name is John"

// 使用靜態方法創建匿名實例
let anonymous = Person.createAnonymous();
anonymous.sayHello(); // 輸出 "Hello, my name is Anonymous"

// 嘗試從實例調用靜態方法
anonymous.createAnonymous(); // 錯誤: anonymous.createAnonymous is not a function
```

在這個版本中:

1. 我們使用 class 關鍵字定義了一個名為 Person 的類別。
2. constructor 方法是建構函數,用於初始化實例屬性 name。
3. sayHello 是一個實例方法,可以被 Person 的實例對象調用。
4. createAnonymous 是一個靜態方法,使用 static 關鍵字定義,只能由類別本身調用,不能被實例對象調用。
5. 在 main.js 中,我們使用 new 關鍵字創建 Person 實例,和使用靜態方法創建匿名實例。

使用類別語法糖,代碼看起來更加簡潔清晰,符合傳統的面向對象編程習慣。但本質上,JavaScript 的類別還是基於原型繼承實現的,只不過語法上更接近其他傳統的面向對象語言。

需要注意的是,靜態方法的使用方式與建構函數完全一樣,都是通過類別名稱直接調用,而不能從實例對象上調用。這一點在使用類別語法糖時沒有任何變化。