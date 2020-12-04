

Suppose there are two files,

```
A
aaa
bbb
ccc
ddd

B
aaa
bbb
eee
fff
```



 How to remove duplicate rows?

```
aaa
bbb
ccc
ddd
eee
fff
```

```
cat A B | sort | uniq
```

How to get intersection?

```
aaa
bbb
```

```
cat A B | sort | uniq -c | awk -F " " '{if($1!=1) print $2;}'  
```

