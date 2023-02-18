---
layout: post
title: leetcode two sum
date: 2023-02-18 12:32 +0900
---

<!--break-->
## index 
--- 
- [index](#index)
- [문제](#문제)
- [풀이](#풀이)
- [recap](#recap)

<br>

## 문제 
--- 
```
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]
Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]
 

Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
```
<br>
<br>

## 풀이 
--- 
<br>
<br>

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(ns, target) {
    const m = {};
    

    for(let i = 0; i < ns.length; i++){
        if(m.hasOwnProperty(ns[i])){
            
            return [i, m[ns[i]]]
        }
        m[target - ns[i]] = i
     
    }
};
```

위 코드는 leet code에 있는 답안을 그대로 가져온 것이다.

분석을 좀 해보자면

input -> [1,2,3]
target -> [4]

이런식으로 입력값이 들어온다고 해보자.

그럼 JS의 객체를 생성한다.

```js
const m = {}
```

input 배열에 대한 반복문을 돌면서
이러한 값을 찾을 것이다.

4에서 1을 빼면 3이다.
m에 3을 key로 value를 i로 저장한다.

첫 순회에서 
```js
m = {'3':0}
```
이렇게 저장된다.

위 저장은 배열을 순회하는 도중 3이라는 값을 만나면
1이라는 값이 있는 위치 정보를 저장한 것이다.

이게 핵심이다.

이제 순회를 돌면서 3과 마주친다면 저장했던 m이라는 객체의
값을 꺼내오면 된다.


## recap 
--- 
<br>
<br>

알고리즘을 손에서 놓으면 안될것 같다
