---
layout: post
title: "Data Structure Day 1"
subtitle: "리트코드"
categories: 코딩테스트
comments: true
tags: 리트코드
---

<br>

취업 1개월차... 출퇴근 4시간의 고통으로 체력이 고갈나서 공부를 못했다 <br>
이대로는 안될거같아서 가볍게 부담없는 알고리즘 문제 풀기!(회사 스택 공부 꾸준히 하기👊) <br>

## 217. Contains Duplicate

[리트코드](https://leetcode.com/problems/contains-duplicate/) <br>

<br>

```javascript
var containsDuplicate = function (nums) {
  const set = new Set(nums);
  return set.size !== nums.length;
};

console.log(containsDuplicate([1, 3, 5, 7, 1]));
```

<br>
<br>

## 53. Maximum Subarray

[리트코드](https://leetcode.com/problems/maximum-subarray/) <br>

<br>

```javascript
// 카데인 알고리즘
// 각각의 최대 부분합은 이전 최대 부분합이 반영된 결과값
// 이전 인덱스가 가질 수 있는 최대 부분합에 현재의 인덱스 값을 더한다면 현재 인덱스가 가질 수 있는 최대 부분합을 구할 수 있음
// 각각의 인덱스 값은 이전 인덱스가 갖고 있는 최대 부분합을 연장할지, 아니면 자신의 값으로 초기화할지 그저 선택
var maxSubArray = function (nums) {
  for (let i = 1; i < nums.length; i++) {
    nums[i] = Math.max(nums[i] + nums[i - 1], nums[i]);
  }
  return Math.max(...nums);
};

console.log(maxSubArray([-2, 1, -3, 4, -1, 2, 1, -5, 4]));
```

<br>
