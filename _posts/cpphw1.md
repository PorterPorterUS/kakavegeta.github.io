---
layout: post
author: Akira
title: "cpp hw1"
tags: cpp

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



### nth_element 

```c++
//Defined in header <algorithm>
void nth_element(RandomIt first, RandomIt nth, RandomIt last, Compare comp);
```

`nth_element`  is a partial sorting algorithm which rearrange the array such that, given comparison function `comp`, `comp(*j, *i) == false` for all `i` in range `[first, nth)` and for all `j` in range `[nth, last)`. 

In other words, all elements in range `[nth, last)` is greater than or equal to `nth` element, and all elements in range `[first, nth)` is less than `nth` element. 

`nth_element` is useful in scenarios such as finding median, the `kth` smallest/largest element and the top`k` smallest/largest elements, for example,

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>
 
int main()
{
    std::vector<int> v{5, 6, 4, 3, 2, 6, 7, 9, 3};
 	int i;
    std::nth_element(v.begin(), v.begin() + v.size()/2, v.end());
    std::cout << "The median is " << v[v.size()/2] << '\n';
    
    std::nth_element(v.begin(), v.begin()+2, v.end());
    std::cout << "The third smallest element is " << v[2] << '\n';
    std::cout << "The three smallest elements are : " << endl;
    for (i = 0; i < 3; ++i) cout << v[i] << " ";
    cout << endl;
 
    std::nth_element(v.begin(), v.begin()+2, v.end(), std::greater<int>());
    std::cout << "The third largest element is " << v[2] << '\n';
    std::cout << "The three largest elements are : " << endl;
    for (i = 0; i < 3; ++i) cout << v[i] << " ";
    cout << endl;
}
```



### String

- `find` finds the first substring equals to the given string, searches from `pos`, and returns the position of substring first appearance. 

  ```
  size_type find(const string &str, size_type pos = 0);
  ```

- `insert` inserts characters into string at given index

  ```
  void insert(size_type index, const string &str);
  ```

- `substr` returns a substring from index `pos` with length `count`

  ```
  string substr(size_type pos = 0, size_type count = npos); // npos == string.size()
  ```

- `push_back` appends the given character to the end of string

  ```
  void push_back(char ch);
  ```

- `erase` removes specified characters from given index and length

  ```
  string &erase(size_type index = 0, size_type count = npos); // npos == string.size()
  ```

- `clear` removes all characters from string and all pointers, references and iterators will be invalidated. 

  ```
  void clear();
  ```



### HZOJ287 and Huffman coding

#### Huffman coding:

Given:

 $$A = (a_1, a_2, ... , a_n)$$, symbol alphabet of size n, 

$$W = (w_1, w_2, ... w_n)$$, symbol weights,

To obtain output codeword $$C(W) = (c_1, c_2, ..., c_n)$$, where $$c_i$$ is the codeword for symbol $$a_i$$, the goal is:

​												$$min$$  $$L(C(W)) = \sum{w_i * length(c_i)}$$

#### HZOJ287

Given:

 $$A = (a_1, a_2, ... , a_n)$$, fruit types of size n, 

$$W = (w_1, w_2, ... w_n)$$, amount of each type fruit,

To obtain minimum energy costs $$E(W)$$, the goal is: 

​												$$min$$  $$E(W) = \sum{w_i * count(a_i)}$$

where $$count(a_i)$$ is how many times $$a_i$$ would be moved. 

Therefore, HZOJ287 is the same problem with Huffman coding. Intuitively, the more one fruit is, the less times it should be moved. 



### Complex Number

```c++
#include <iostream>
#include <iomanip>

using namespace std;

class Complex {
public:

    Complex() = delete;

    Complex(double re, double im): re(re), im(im){}

    Complex(const Complex &a) {
        im = a.im, re = a.re;
    }

    Complex &operator=(const Complex &a) {
        im = a.im, re = a.re;
        return *this;
    } 

    double Re() {
        return this->re;
    }

    double Im() {
        return this->im;
    }

    ~Complex() = default;
private:
    double re, im;
    friend Complex operator+(const Complex &a, const Complex &b);
    friend Complex operator-(const Complex &a, const Complex &b);
    friend Complex operator*(const Complex &a, const Complex &b);
    friend Complex operator/(const Complex &a, const Complex &b);
    friend ostream &operator<<(ostream &out, const Complex &a);
};

Complex operator+(const Complex &a, const Complex &b) {
    Complex c(a.re + b.re, a.im + b.im);
    return c;
}

Complex operator-(const Complex &a, const Complex &b) {
    Complex c(a.re - b.re, a.im - b.im);
    return c;
}


Complex operator*(const Complex &a, const Complex &b) {
    double re = (a.re * b.re) - (a.im * b.im);
    double im = a.re * b.im + a.im * b.re;
    Complex c(re, im);
    return c;
}

Complex operator/(const Complex &a, const Complex &b) {
    double re = (a.re * b.re + a.im * b.im) / (b.re * b.re + b.im + b.im);
    double im = (a.re * b.re - a.im * b.im) / (b.re * b.re + b.im + b.im);
    Complex c(re, im);
    return c;
}

ostream &operator<<(ostream &out, const Complex &a) {
    if(a.im > 0) 
        out << setprecision(2) << a.re << " + " << setprecision(2) << a.im << "i";
    else if (a.im == 0)
        out << setprecision(2) << a.re;
    else 
        out << setprecision(2) << a.re << " - " << setprecision(2) << -a.im << "i";
    return out;
}

int main() {
    // constructor;
    Complex a(1,2), b(3,4);
    // copy constructor;
    Complex c = b;
    // copy assignment operator;
    b = c;
    // <<
    cout << "a : " << a << endl;
    cout << "b : " << b << endl;
    // operators
    cout << "a + b : " << a + b << endl;
    cout << "a - b : " << a - b << endl;
    cout << "a * b : " << a * b << endl;
    cout << "a / b : " << a / b << endl;
}

```

