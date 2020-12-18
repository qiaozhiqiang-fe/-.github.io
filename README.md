# 前端算法题-qiaozhi
**通过字符串、数组等经常遇到的面试题进行分析解答，希望能够帮助到大家；欢迎建议、评论**
## 无重复字符串的最大长度计算
举例1： 'abcdadda' 'abcd' 4

举例2：'abababab' 'ab'   2
#### 解法一 （维护数组）
```
const notRepeatMaxLength = (str) => {
    if(typeof str !== 'string') return 0;
    const arr = [];
    let max = 0;
    for(let i=0;i<str.length;i++){
        const index = arr.indexOf(str[i]);
        if(index !== -1){
            arr.splice(0, index+1);
        }
        arr.push(str[i]);
        max = Math.max(arr.length, max); 
    }
    return max;
}
```

## 判断是不是回文字符串
题目分析：字符串正反都是相等 
例如： '123321', 'asdfdsa'
#### 解法一
```
const plalindrome1 = (str)=>{
  if(typeof str !== 'string') return false;
    const len = str.length;
    let isPlalindrome = true;
    for(let i=0;i<len-i-1;i++){
        if(str[i] !== str[len-i-1]){
            isPlalindrome = false;
        }
    }
    return isPlalindrome;
}
```
#### 解法二
```
const plalindrome2 = (str)=>{
    if(typeof str !== 'string') return false;
    if(str.split('').reverse().join('') === str) return true;
    return false;
}
```
## 字符串中出现最多的字符个数
例如： 'abcddaddd' // d 4

思路： 利用对象来计数
```
const mostString = (str) => {
    const obj = {};
    for(let i=0;i<str.length;i++){
        if(obj[str[i]]){
            obj[str[i]]++
        } else {
            obj[str[i]] = 1;
        }
    }
    return Math.max.apply(null,Object.values(obj));
    // 或者
    return Math.max(...Object.values(obj));
}
```
## 判断一个字符串是否为括号正确闭合
例如："{[()]}", "(){()}[]"

错误列子： "((])", "{([)]}"

思路：利用栈结构，依次循环查找成对括号
```
const computed = (str) => {
  if (typeof str !== 'string') return false;
  const arr = [];
  const obj = {
    '(': ')',
    '[': ']',
    '{': '}'
  };
  for (let i = 0; i < str.length; i++) {
    if (obj[str[i]]) {
      arr.push(str[i]);
    } else if (str[i] !== obj[arr.pop()]) {
      return false;
    }
  }
  return !arr.length;
}
```
## 依次删除字符串中相邻相等字符串
例子："abbac" // c "abbac"=>"aac"=>"c"

思路：建立一个空数组，利用栈结构依次消除将要进入数组的字符串和数组最后一个字符串相等元素；
```
const removeRepeat = (str) => {
  if (typeof str !== 'string') return false;
  const arr = [];
  for (let i = 0; i < str.length; i++) {
    const s = arr.pop();
    if (str[i] !== s) {
      if (s) arr.push(s);
      arr.push(str[i]);
    }
  }
  return arr.join('');
}
```
## 英文句翻转且去除空格
例子： "i miss  you " // "you miess i"
```
const turn = (str) => {
	if(typeOf str !== "string") return false;
    return str.split(" ").filter(s => !!s).reverse().join(' ');
}
```
## 合并两个有序数组
例如：[1,2,4,5] [3,5,7,9] // [1,2,3,4,5,5,7,9]

思路：使用双数组总长度循环和各个数组长度，依次从末尾对比两个数组的最大值，谁大unshift进入新数组，并且当前数组长度减1；
```
const mergeOderArray = (arr1, arr2)=>{
    let arr = [];
    let len1 = arr1.length;
    let len2 = arr2.length;
    const len = len1+len2;
    if(!len1) return arr2;
    if(!len2) return arr1;
    for(let i=0;i<len;i++){
        if(len1 === 0 && len2 === 0) return;
        if(len1 === 0){
            arr = [...arr2.slice(0, len2), ...arr];
            break;
        }
        if(len2 === 0){
            arr = [...arr1.slice(0, len1), ...arr];
            break;
        }
        arr1[len1-1]>arr2[len2-1] ? arr.unshift(arr1[--len1]) : arr.unshift(arr2[--len2]);
    }
    return arr;
}
```
边界分析：
- arr1长度为0直接返回arr2
- arr2长度为0直接返回arr1
- 循环过程中出现len1或者len2为0时处理
## 两个数组求并集、交集、差集
例如：[1,2],[1,3] // 并集[1,2,3]、交集[1]、list1相对list2差集[2,3]
```
// 并集
const unionSet = (list1, list2) => [...new Set([...list1, ...list2])]
// 交集
onst intersectionSet = (list1, list2) => [...new Set(list1.filter(item=>{
    return list2.includes(item);
}))]
// 差集
const differenceSet = (list1, list2) => [...new Set(list1.filter(item=>{
    return !list2.includes(item);
}))]
```
## 数组扁平化
例如：[1,2,[2,3,[3,4],6],6] // [1,2,2,3,3,4,6,6]
#### 解法一 (es6方法)
```
arr.flat(Infinity)
```
#### 解法二 (字符串或者分解数组然后组合方式)
```
arr.join(",").split(',').map(item=> Number(item));
// 或者
arr.toString().split(',').map(item=> Number(item))
```
#### 解法三 (结构数组方式)

注：判断数组的方法
```
Array.isArray(arr) // true
Object.prototype.toString.call(arr) // [object, Array]
arr.constructor// Array
arr instanceof Array // true
```
```
const flatten = (arr)=>{
    while(arr.some(item=> Array.isArray(item))){
        arr = [].concat(...arr);
    }
    return arr;
}
或者
let index = 1;
const flatten = (arr, num)=>{
    if(index>num){
        return arr;
    }
    console.log(index)
    if(arr.some(item=>Array.isArray(item))){
        arr = [].concat(...arr);
        index++;
        return flatten(arr, num);
    } else {
        return arr;
    }
}
console.log(flatten(arr, 1)); // [1,2,2,3,[3,4],6,6]
```
## 数组中三个数字求和为0的组合
例如：[-1, 0, 1, 2, -1, -4] // [[-1, -1, 2],[-1, 0, 1]];

备注：三层循环方式就不赘述了

思路：先对数组排序，通过利用双指针方式，循环数组当前坐标i，从两头开始向中间靠拢l=i+1,r=length-1，求出三个坐标的和，大于0说明r大，r--后继续；反之i小，i++
```
const threeSum = (nums)=> {
    nums = nums.sort((a,b)=> a-b);
    const arr = [];
    const len = nums.length;
    if(len<3) return arr;
    for(let i=0;i<len;i++){
        let l = i+1;
        let r = len-1;
        if(nums[i]>0) break;
        if(nums[i] === nums[i-1]) continue;
        while(l<r){
            const s = nums[i]+nums[l]+nums[r];
            if(s>0){
                r--;
            } else if(s<0){
                l++;
            } else {
                arr.push([nums[i],nums[l],nums[r]]);
                while (nums[l] === nums[l+1]) l++;
                while (nums[r] === nums[r-1]) r--;
                l++;
                r--;
            }
        }
    }
    return arr;
}
```
## 斐波拉数列
示例： [1,1,2,3,5,8,13] //n=7 return 13
#### 方法一（递归）
```
const fib1 = (n)=>{
    if(n=== 1|| n===2){
        return 1;
    }
    return fib1(n-1)+fib1(n-2);
}
```
时间复杂度为O(2^n)，空间复杂度为O(n)
#### 方法二（变量替换）
```
 const fib2 = (n)=>{
    let num = 1,
    pre = 0,
    next = 1;
    for(let i=1;i<n;i++){
        num = pre+next;
        pre = next;
        next = num;
    }
    return num;
}
```
时间复杂度为O(n)，空间复杂度为O(1)
#### 方法三（数组递进）
```
const fib3 = (n)=>{
    let arr = [1,1];
    for(let i=2;i<n;i++){
        arr[i] = arr[i-1]+arr[i-2];
    }
    return arr[n-1];
}
```
时间复杂度为O(n)，空间复杂度为O(1)


