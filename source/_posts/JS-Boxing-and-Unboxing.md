title: '[JS]  Boxing and Unboxing '
author: Wei Wei
tags:
  - Javascript
categories:
  - Tmp
date: 2024-04-23 01:25:00
---
## 參考資料
- [JavaScript Boxing and Unboxing](https://www.naukri.com/code360/library/javascript-boxing-and-unboxing)

---

<!-- more -->
在練習原型的時候問了Claude以下問題:

我們在js中使用const n=3
為什麼會跟Number建構函式有關?
這是因為是內建的關係所以在中間自行轉換了嗎?
那為什麼n又跟new Number(3)所產生的內容不同呢

---

您提出了一個很好的問題,關於基本數據類型與它們對應的包裝類別對象之間的區別。

當我們使用 const n = 3 這樣的語法時,實際上是創建了一個基本數據類型的數值 3,而非實例化一個 Number 對象。這種基本數據類型也被稱為原始值(primitive value)。

而 new Number(3) 則是使用 Number 建構函數創建了一個 Number 對象的實例。這個實例擁有更多的屬性和方法,例如 toString()、valueOf() 等。

之所以會有這樣的區別,是因為 JavaScript 為了方便操作基本數據類型,在需要時會自動在原始值和對應的包裝對象實例之間進行隱式轉換。這個過程被稱為boxing和unboxing。

例如,當我們執行 const n = 3; n.toString(); 這樣的語句時,JavaScript 引擎會臨時創建一個 Number 對象實例,並將原始值 3 包裝在該實例中,從而允許我們調用 toString() 方法。一旦該操作完成,臨時創建的對象實例就會被銷毀。

但是,如果我們直接使用 new Number(3) 創建的是一個持久的對象實例,它與基本數據類型 3 是不同的。對象實例還擁有一些其他內部屬性,因此在某些運算中的表現會與基本數據類型有所不同。

舉個例子,如果我們比較 new Number(3) === 3 的結果會是 false,因為一個是對象實例,另一個是基本數據類型,它們在內存中的存儲方式不同。而 3 === 3 的結果則是 true。

總的來說,JavaScript 的設計是為了方便操作基本數據類型,通過自動boxing/unboxing的機制來提供對應對象的方法和屬性。但開發人員仍需注意基本類型和對象實例之間的區別,避免出現一些潛在的問題。大多數情況下,我們只需使用基本數據類型即可,除非確實需要對象實例的特殊功能和屬性。