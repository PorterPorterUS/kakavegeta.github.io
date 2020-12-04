---
layout: post
author: Akira
title: "size() return unsigned int --- record a stupid bug I made"
tags: c++ leetcode
---


### One notification about .size() in c++

Today I made a stupid mistake when doing leetcode exercises... Just a record to avoid falling into the pit again...

The problem is: 

##### 1673. Find the Most Competitive Subsequence

Given an integer array `nums` and a positive integer `k`, return *the most **competitive** subsequence of* `nums` *of size* `k`.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence `a` is more **competitive** than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number **less** than the corresponding number in `b`. For example, `[1,3,4]` is more competitive than `[1,3,5]` because the first position they differ is at the final number, and `4` is less than `5`.



It an interesting problem, we need to put a less number to front as far as possible, and in the mean time, you should pay attention to the length of the results. A classic monotonic queue/stack problem, and the answer is simple:

```c++
class Solution {
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) {
        vector<int> ans;
        for (int i=0; i<nums.size(); ++i) {
            while (!ans.empty() && (ans.back() > nums[i]) && ans.size() + nums.size()-i-1 >= k) {
                ans.pop_back(); 
            }
            ans.push_back(nums[i]);
        }
        
        while (ans.size() > k) ans.pop_back();

        return ans;
    }
};
```



But you know what mistake I made? I wrote the while condition as:

```c++
while (!ans.empty() && (ans.back() > nums[i]) && nums.size()-i-1 >= k - ans.size())
```

Seems no problem. However, since `ans.size()` is  `unsigned int`,  and `k` is a `signed int`. So what happens? `k` is converted to unsigned int. Whoops! 

 
