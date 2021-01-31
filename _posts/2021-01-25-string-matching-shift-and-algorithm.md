---
layout: post
author: Akira
title: "Shift-And Algorithm"
tags: "string matching"
categories: ["string matching"]
---
{% include lib/mathjax.html %}



Today is another string matching algorithm, shift-and, which is efficient, elegant, and simple! Behind this algorithm is bit manipulation.  

Consider following text(in length M) and pattern(in length N):

```
T: abcdefegdjkl
P: defegd
```

Shift-and algorithm firstly encodes pattern string into a table, where each distinct character is mapped into an int value. Each value represents the position of the corresponding character's appearance. Formally, `d` maps each char into a N-bit value, $d_{N-1}, d_{N-2}, d_{N-3}, ... d_0$. For each bit, 1 if the character at that index(0-based) and 0 if not. 

```
d['d'] = 100001
d['e'] = 001010
d['f'] = 000100
d['g'] = 010000
```

After encoding pattern, we iterate each character in text. Here comes the most magic part of shift-and algorithm, simply one line code:

```
// p is initially 0
For each character T[i] in text, update p: 
p = (p << 1 | 1) & d[T[i]]
```

What `p` is? It is the state of matching at T[i]. Too abstract, let's finish matching one by one step.

```
^
a b c d e f e g d j k l
d['a']: 0 0 0 0 0 0 
p     : 0 0 0 0 0 0 

  ^
a b c d e f e g d j k l
d['b']: 0 0 0 0 0 0 
p     : 0 0 0 0 0 0 

    ^
a b c d e f e g d j k l
d['c']: 0 0 0 0 0 0 
p : 0 0 0 0 0 0 

      ^
a b c d e f e g d j k l
d['d']            : 1 0 0 0 0 1 
p<<1 | 1          : 0 0 0 0 0 1
p<<1 | 1 & d['e'] : 0 0 0 0 0 1

        ^
a b c d e f e g d j k l
d['e']            : 0 0 1 0 1 0 
p<<1 | 1          : 0 0 0 0 1 1
p<<1 | 1 & d['e'] : 0 0 0 0 1 0

          ^
a b c d e f e g d j k l
d['f']            : 0 0 0 1 0 0 
p<<1 | 1          : 0 0 0 1 0 1
p<<1 | 1 & d['e'] : 0 0 0 1 0 0

            ^
a b c d e f e g d j k l
d['e']            : 0 0 1 0 1 0
p<<1 | 1          : 0 0 1 0 0 1
p<<1 | 1 & d['e'] : 0 0 1 0 0 0

              ^
a b c d e f e g d j k l
d['g']            : 0 1 0 0 0 0
p<<1 | 1          : 0 1 0 0 0 1
p<<1 | 1 & d['e'] : 0 1 0 0 0 0

                ^
a b c d e f e g d j k l
d['d']            : 1 0 0 0 0 1
p<<1 | 1          : 1 0 0 0 0 1
p<<1 | 1 & d['e'] : 1 0 0 0 0 1

Finished
```

Now get what `p` means? Suppose `j-th` bit is 1(from right to left), it states a fact that, at T[i], pattern can be matched from `0` to `j`.  Therefore, as long as $p_{N-1}$ is 1, we get answer!

Here is the code:

```c++
int shift_and(const char *s, const char *t) {
    int m = strlen(s), n = 0;
    int d[256] = {0};
    for (int i=0; t[i]; ++i, ++n) {
        d[t[i]] |= 1 << i;
    }
    
    int p = 0;
    for (int i=0; s[i]; ++i) {
        p = (p << 1 | 1) & d[s[i]];
        if (p & (1 << (n-1))) return i - n + 1;
    }
    return -1;
}

```

