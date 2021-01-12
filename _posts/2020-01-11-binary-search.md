---
layout: post
author: Akira
title: "Binary Search in details"
tags: algorithm leetcode
---

{% include lib/mathjax.html %}


<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: [
      "MathMenu.js",
      "MathZoom.js",
      "AssistiveMML.js",
      "a11y/accessibility-menu.js"
    ],
    jax: ["input/TeX", "output/CommonHTML"],
    TeX: {
      extensions: [
        "AMSmath.js",
        "AMSsymbols.js",
        "noErrors.js",
        "noUndefined.js",
      ]
    }
  });
</script>



Binary search algorithm can be confusing in application. 

#### Normal case: no replicates

Since no replicates, there is only one target we need to find.  

```c
int binary_search(int *a, int n, int target) {
    int l = 0, r = n-1, mid; 
    while (l <= r) {
        mid = l + (r-l)/2;
        if (a[mid] == target) return mid;
        if (a[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}
```



However, in many cases there are replicates in array. 

#### First element

We abstract the array as 00000111111. To find the left edge, that is, the first 1, when we meet one 1, we cannot simply return the index. Rather, we need to shrink right boundary.

```c
// l < r
int binary_search(int *a, int n, int target) {
    int l = 0, r = n, mid; // set r as n, so the condition in while loop is l < r
    while (l < r) {
        mid = (l + r) / 2;
        if (a[mid] == 0) l = mid + 1;
        else r = mid;
    }
    return r
}

// l <= r
int binary_search(int *a, int n,, int target) {
    int l = 0, r = n-1, mid;
    while (l <= r) {
       	mid =  (l + r) / 2;
        if (a[mid] == 0) l = mid + 1;
        else r = mid - 1;
    }
    return r + 1;
}
```

 If all elements are 0, binary search will return `n` , which means there is no desirable value in array. 



