---
nav:
  title: 力扣
---

# 🔥LeetCode 热题 HOT 100

[![](https://static.leetcode-cn.com/cn-mono-assets/production/main/assets/logo-dark-cn.c42314a8.svg)](https://leetcode-cn.com/problemset/leetcode-hot-100/)

## 1、两数之和

```jsx | inline
import React from 'react';
export default () => (
  <div>
    难度： <div className="label round label-success">简单</div>
  </div>
);
```

给定一个整数数组 nums  和一个目标值 target，请你在该数组中找出和为目标值的那   两个   整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

### 暴力解法

双层循环：

- **时间复杂度 O(n^2)**

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  for (let i = 0; i < nums.length - 1; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
};

var nums = [2, 7, 11, 15];
var target = 9;
var result = twoSum(nums, target);
console.log(result); // [0, 1]
```

### 哈希表

- 在遍历的同时，记录一些信息，以省去一层循环，这是“以空间换时间”的想法
- 需要记录已经遍历过的数值和它对应的下标，可以借助哈希表实现
- **时间复杂度 O(n)**

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */

function twoSum(nums, target) {
  let map = new Map();
  map.set(nums[0], 0);
  for (let i = 1; i < nums.length; i++) {
    if (map.has(target - nums[i])) {
      return [map.get(target - nums[i]), i];
    }
    map.set(nums[i], i);
  }
}

var nums = [2, 7, 11, 15];
var target = 9;
var result = twoSum(nums, target);
console.log(result);
```

哈希表的奇技淫巧写法，不过都没有绕过 **哈希表**

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  let obj = {};
  let cache = [];
  for (let i = 0; i < nums.length; i++) {
    let a = nums[i];
    let diff = target - nums[i];
    if (obj[a] !== undefined) {
      return (cache = [obj[a], i]);
    } else {
      obj[diff] = i;
    }
  }
};

var twoSum = function(nums, target) {
  const map = {};
  const len = nums.length;
  for (let i = 0; i < len; i++) {
    const targetNum = target - nums[i];
    if (targetNum in map) return [map[targetNum], i];
    map[nums[i]] = i;
  }
};
```

## 7. 整数反转

```jsx | inline
import React from 'react';
export default () => (
  <div>
    难度： <div className="label round label-success">简单</div>
  </div>
);
```

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

```
输入: 123
输出: 321
```

示例 2:

```
输入: -123
输出: -321
```

示例 3:

```
输入: 120
输出: 21
```

注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231, 231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

### 反转字符串

```js
var reverse = function(x) {
  let num = Math.abs(x);
  let str = num + '';
  str = str
    .split('')
    .reverse()
    .join('');
  let result = parseInt(str);

  if (x > 0) {
    result = result > Math.pow(2, 31) - 1 ? 0 : result;
  } else {
    result = result < -Math.pow(2, 31) ? 0 : result;
  }
  return result;
};

console.log(reverse(-123));
```

### 取余法

这中写法比上述暴力的 “**字符串反转法**” 在思维上略高级一些

```js
function reverse(x) {
  let result = 0;
  let num = Math.abs(x);
  while (num > 0) {
    result = result * 10 + (num % 10);
    num = Math.floor(num / 10);
  }
  if (x > 0) {
    result = result > Math.pow(2, 31) - 1 ? 0 : result;
  } else {
    result = result < -Math.pow(2, 31) ? 0 : -result;
  }
  return result;
}
```

### 数学法

```js
var reverse = function(x) {
  let result = 0;
  while (x !== 0) {
    result = result * 10 + (x % 10);
    x = (x / 10) | 0;
  }
  return (result | 0) === result ? result : 0;
};

console.log(reverse(123)); // 321
```

这中写法很高级，相比我业务型的思路，这种写法确实高大上了！使用了模运算、位运算

> [位运算](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Expressions_and_Operators#%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6)： 按位或 OR a | b 在 a,b 的位表示中，每一个对应的位，只要有一个为 1 则返回 1， 否则返回 0. ———— MDN

## 20、有效的括号

```jsx | inline
import React from 'react';
export default () => (
  <div>
    难度： <div className="label round label-success">简单</div>
  </div>
);
```

给定一个只包括 '('，')'，'{'，'}'，'['，']'  的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

```
输入: "()"
输出: true
```

示例  2:

```
输入: "()[]{}"
输出: true
```

示例  3:

```
输入: "(]"
输出: false
```

示例  4:

```
输入: "([)]"
输出: false
```

示例  5:

```
输入: "{[]}"
输出: true
```

### Stack 方法

首先方栈中放置元素，匹配到元素的另一半则将原有元素 `pop()`

最后，根据栈是否被清空判断时否全部匹配

```js
var isValid = function(s) {
  if (s % 2 === 0) {
    return false;
  }
  let stack = [];
  for (let i = 0; i < s.length; i++) {
    if (s[i] === '(') {
      stack.push(')');
    } else if (s[i] === '[') {
      stack.push(']');
    } else if (s[i] === '{') {
      stack.push('}');
    } else if (!stack.length || s[i] !== stack.pop()) {
      return false;
    }
  }
  return !stack.length;
};

console.log(isValid('()]')); // false
console.log(isValid('{()')); // false
console.log(isValid('{()]')); // false
console.log(isValid('({[]})')); // true
console.log(isValid('(){}[]')); // true
```

### Stack + 哈希表

```js
var isValid = function(s) {
  const n = s.length;
  if (n % 2 === 1) {
    return false;
  }
  const pairs = new Map([
    [')', '('],
    [']', '['],
    ['}', '{'],
  ]);
  const stk = [];
  s.split('').forEach(ch => {
    if (pairs.has(ch)) {
      if (!stk.length || stk[stk.length - 1] !== pairs.get(ch)) {
        return false;
      }
      stk.pop();
    } else {
      stk.push(ch);
    }
  });
  return !stk.length;
};

console.log(isValid('()]')); // false
console.log(isValid('{()')); // false
console.log(isValid('{()]')); // false
console.log(isValid('({[]})')); // true
console.log(isValid('(){}[]')); // true
```

**Stack + 哈希表** 复杂度分析

**时间复杂度**：O(n)，其中 n 是字符串 s 的长度。

**空间复杂度**：O(n+∣Σ∣)，其中 |Σ| 表示字符集，本题中字符串只包含 6 种括号，∣Σ∣=6。栈中的字符数量为 O(n)，而哈希映射使用的空间为 O(∣Σ∣)，相加即可得到总空间复杂度。
