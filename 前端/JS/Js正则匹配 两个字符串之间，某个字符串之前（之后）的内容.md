**1、js截取两个字符串之间的内容：**  
var str = "aaabbbcccdddeeefff";
str = str.match(/aaa([\s\S]*?)fff/)[1];
console.log(str);//结果bbbcccdddeee

**2、js截取某个字符串前面的内容：**
var str = "aaabbbcccdddeeefff";
str = str.match(/(\S*)fff/)[1];
console.log(str);//结果aaabbbcccdddeee

**3、js截取某个字符串后面的内容：**
var str = "aaabbbcccdddeeefff";
str = str.match(/aaa(\S*)/)[1];
console.log(str);//结果bbbcccdddeeefff

**4、js全局截取两个字符串之间的内容：**
var str = 'abcdabcdabcd'
str = str.match(/a([\s\S]*?)d/g); 
var res = str.toString().replace(/a/g,'').replace(/d/g,'');
console.log(res); //结果bc,bc,bc