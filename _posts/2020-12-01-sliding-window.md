### Sliding Window in details



A question first: given an array, what is the time complexity of generating all subarrays? Obviously, O(n^2).   But what if we do not need to compute all subarrays but some of them which satisfy our specific requirement? The answer is, O(n), but with sliding window(or two pointers) trick. 

Why sliding window can down optimize time complexity?  In brief, it reduces the cases we need to test. Let's start with a simple example.

#####  [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

Is this problem can be solved by sliding window?  The answer is no. But how about this one:

##### [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

The answer is yes. 

Firstly let's see why 209 can be solved by sliding window.  Given `s = 7, nums = [2,3,1,2,4,3]` , denote `left` and `right`  are two pointers. Say not we progress to a case`left = 0, right = 4`, which is a candidate answer because the sum of this subarray is 8. If we do this problem in brutal force,  we need to continue move `right` forward to end and check whether each case satisfies criterion. But wait, is that necessary? For each `left = 0, right > 4`,  although it satisfies criterion (>=7) , it cannot be in smaller size than case `left = 0, right = 4` . Therefore, there is no need to move `right` ahead. Instead, the next case we should test is `left = 1, right = 4` . And this is the magic of why sliding window will reduce time complexity: it reduces the space we need to search! 

Now it is easy to explain why 560 cannot. Given `nums = [1,1,1,-1], k = 2` , and we progress to a candidate case `left  = 0, right = 1`. After we find a candidate one, can we stop moving right and begin move left? Absolutely no, because you will lose another candidate case `left = 0, right = 3`. Therefore, you face an searching space which cannot be shrank. 

In fact, 560 does not specify positive value. If it says every element is positive, then it can be solved by sliding window!

After figure out the mechanism behind, let's see some interesting problem which can be solved by sliding window. 



#### [1248. Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/) 

behind is subarray sum similar to 560, change odd to 1, even to 0.  But why this can be solved by sliding window? Because elements are >= 0! 

