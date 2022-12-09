代码如下，计算投硬币的概率：

```ts
'use strict';

  

// play num 次数，其中是否有连续long个重复，模拟投硬币

function Play(num: Number, long: Number): Boolean {

let i = 1;

let arr = [];

let count = 0;

let prev = null;

  

for (let i = 0; i < num; i++) {

let curr = Math.random();

curr = curr < 0.5 ? 0 : 1;

  

// 第一个元素

if (arr.length === 0) {

arr.push(curr);

count = 1;

prev = curr;

} else {

// 第二个元素开始

if (curr === prev) {

arr.push(curr);

count++;

// 判断是否达到了需要的次数

if (count >= long) {

return true

}

} else {

arr = [];

arr.push(curr);

count = 1;

prev = curr;

}

}

}

  

return false

}

  

// 计算概率 多少天，每天多少次，假设连续输的最长的次数

function Run(day: number, num: number, l: number): String {

  

let trueCount = 0, falseCount = 0, i = 1;

  

while (i <= day) {

if (Play(num, l)) {

trueCount++

} else {

falseCount++

}

i++;

}

  

console.log(trueCount);

console.log(falseCount);

  

return `${day}中有${trueCount}天至少连续${l}次输， \n每次损失的本金最少是： ${Math.pow(2, l) * 10}`

}

  
  

console.log(Run(365, 60 * 8 * 2, 16));

```