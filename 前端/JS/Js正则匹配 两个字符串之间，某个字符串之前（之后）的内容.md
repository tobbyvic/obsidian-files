JS截取两个字符串之间的内容

```js
var str = "aaabbbcccdddeeefff";
str = str.match(/aaa([\s\S]*?)fff/)[1];
console.log(str);//结果bbbcccdddeee
```

JS截取某个字符串前面的内容

```js
var str = "aaabbbcccdddeeefff";
str = str.match(/(\S*)fff/)[1];
console.log(str);//结果aaabbbcccdddeee
```

### JS截取某个字符串后面的内容

```js
var str = "aaabbbcccdddeeefff";
str = str.match(/aaa(\S*)/)[1];
console.log(str);//结果bbbcccdddeeefff
```

### JS全局截取两个字符串之间的内容

```js
var str = 'abcdabcdabcd'
str = str.match(/a([\s\S]*?)d/g); 
var res = str.toString().replace(/a/g,'').replace(/d/g,'');
console.log(res); //结果bc,bc,bc
```