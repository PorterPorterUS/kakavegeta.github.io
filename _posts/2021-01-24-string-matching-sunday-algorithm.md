Sunday algorithm is a very fast algorithm which is, however, surprisingly easy to implement.

Take an example. Consider following text(in length M) and pattern(in length N):

```
T: abcdeghdefjkl
P: def
```

Let's try to match pattern with text in naive way:

```
// n is the position of unmatching firstly
// y is the position of first matching of whole pattern string

n
a b c d e g h d e f j k l
d e f

  n
a b c d e g h d e f j k l
  d e f

...
          n
a b c d e g h d e f j k l
      d e f

...
              y    
a b c d e g h d e f j k l
              d e f

```

The time complexity is O(M*N), since every time we meet an unmatched character, and we need to move pattern one character forward and begin a new loop which iterates all characters in pattern to see whether they are matched in text.



But wait. Does moving one character forward is the only choice? Absolutely not! And how to come up with an efficient strategy to moving pattern forward is the essence of many string matching algorithms. Let's see how Sunday algorithm deal with this.

```
n     
a b c d e g h d e f j  k  l
d e f
0 1 2 3 4 5 6 7 8 9 10 11 12

'a' is unmatched at 0, so at least we have to move one char forward. Now the last character in text need to be matched is 'd' at index 3. To match 'd', pattern need to forward 3 chars (3 comes from fact that 'd' is the the 3rd count backward):

          n     
a b c d e g h d e f j  k  l
      d e f
0 1 2 3 4 5 6 7 8 9 10 11 12.

Now 'd'(3) and 'e'(4) are matched, but 'g' is unmatched. Again, investigate whether 'h' can be matched. Notice that there is no 'h' in pattern, we need to move pattern 4 chars (4 comes from the fact that 'h' does not appear in pattern, so it can be counted backward as 4th element in pattern) to index 7 , the next position of 'h'. Aha, we find answer!
              y     
a b c d e g h d e f j  k  l
              d e f
0 1 2 3 4 5 6 7 8 9 10 11 12.
```

So the core of Sunday algorithm is recording each pattern character's index (1 based) count backward. Based on this number, we decide to move pattern how many chars forward. 

Here is the code:

```c
int Sunday(const char *s, const char *t) {
    int m = strlen(s), n = strlen(t);
    int table[256];
    for (int i=0; i<256; ++i) table[i] = n+1;
    for (int i=0; t[i]; ++i) table[t[i]] = n - i;
    
    for (int i = 0; i < m; i += table[s[i+n]]) {
        int flag = 1;
        for (int j = 0; t[j]; ++j) {
            if (t[j] == s[i + j]) continue;
            flag = 0;
            break;
        }
        if (flag) return i;
    } 
    return -1;
}
```



