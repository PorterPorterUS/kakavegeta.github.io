---
layout: post
author: Akira
title: "Simple Implement of Class Template std::function"
tags: c++

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

`std::function` is a function wrapper, and it's instance is able to store, copy, and invoke any CopyConstructible Callable target, such as functions, lambda expression, bind expressions, or other function objects, as well as pointers to member functions and pointers to data members. 

This post implements a similar template class `function<>` which can store, copy and invoke any function object. It is an review and practice for how to build template class and how to interpret polymorphism. 

For one thing, our task is to build a template class which can act as a wrapper as below:

```c++
// we define a function and a class which overriding function call operator
int add(int a, int b) {
    return a + b;
} 

class Add {
public:
    int operator()(int a, int b) {
        return a + b;
    }
};

int main() {
    Add add_class;
    function<void(int, int)> f1 = add;
    function<void(int, int)> f2 = add_class;
    std::cout << f1(1, 2) << " " f2(3, 4) << std::endl;
    return 0;
}
```

Here polymorphism comes in! So we need to build a  class `Base` with a **pure virtual** function `run()` which is used to bind function and class overriding `()` operator. Why **pure virtual** function? Since as we call `function<void(int ,int)> f1 = add`, we want `run()` goes with object `add` instead of class `function` .

```c++
template<typename T, typename ...ARGS>
class Base {
public:
    virtual T run(ARGS...) = 0;
};
```

Then let's cerate a derived class from `Base` which binds to functions. The reason of using `forward<>` is that we need template class know exactly what we pass, without deducing by it self.

```c++
template<typename T, typename ...ARGS>
class Func : public Base<T, ARGS...> {
public:
    typedef T (*FUNC_T)(ARGS...);
    Func(FUNC_T func) : func(func) {}
    T run(ARGS ...args) override {
       return func(std::forward<ARGS>(args)...);
    }
private:
    FUNC_T func;
};
```

Similarly, another derived class which binds to class overriding `()` operator, or FunctionObject 

```c++
template<typename C, typename T, typename ...ARGS>
class FuncObj : public Base<T, ARGS...> {
public:
    FuncObj(C &&obj) : obj(obj) {}
    T run(ARGS ...args) override {
       return obj(std::forward<ARGS>(args)...);
    }
private:
    C obj; // std::function<> make copy instead of reference
};
```

Now let's create class template `function`

```c++
template<typename T, typename ...ARGS> class function;
template<typename T, typename ...ARGS>
class function<T(ARGS...)> {
public:
    function(T (*ptr)(ARGS...)) : fptr(new Func<T, ARGS...>(ptr)) {}

    template<typename C>
    function(C obj) : fptr(new FuncObj<C, T, ARGS...>(obj)) {};

    T operator()(ARGS ...args) {
        return fptr->run(std::forward<ARGS>(args)...);
    }
private:
    Base<T, ARGS...> *fptr;
};
```

It's done!



all codes:

```c++
#include <iostream>
#include <functional>

template<typename T, typename ...ARGS>
class Base {
public:
    virtual T run(ARGS...) = 0;

};

template<typename T, typename ...ARGS>
class Func : public Base<T, ARGS...> {
public:
    typedef T (*FUNC_T)(ARGS...);
    Func(FUNC_T func) : func(func) {}
    T run(ARGS ...args) override {
       return func(std::forward<ARGS>(args)...);
    }
private:
    FUNC_T func;
};

template<typename C, typename T, typename ...ARGS>
class FuncObj : public Base<T, ARGS...> {
public:
    FuncObj(C &&obj) : obj(obj) {}
    T run(ARGS ...args) override {
       return obj(std::forward<ARGS>(args)...);
    }
private:
    C &obj;
};

template<typename T, typename ...ARGS> class function;
template<typename T, typename ...ARGS>
class function<T(ARGS...)> {
public:
    function(T (*ptr)(ARGS...)) : fptr(new Func<T, ARGS...>(ptr)) {}

    template<typename C>
    function(C obj) : fptr(new FuncObj<C, T, ARGS...>(obj)) {};

    T operator()(ARGS ...args) {
        return fptr->run(std::forward<ARGS>(args)...);
    }
private:
    Base<T, ARGS...> *fptr;
};

void add(int a, int b) {
    std::cout << "normal function " << std::endl;
}

class Add {
public:
    Add() : x{0} {};
    void operator()(int a, int b) {
        x += a + b;
        std::cout << "class object : " << x  << std::endl;
        return;
    }
    int x;
};

int main() {
    Add add_obj;
    std::function<void(int, int)> f1 = add;
    std::function<void(int, int)> f2 = add_obj;
    f1(1, 2);
    f2(3, 4);
    f2(5, 6);
    function<void(int, int)> f3 = add;
    function<void(int, int)> f4 = add_obj;
    f3(1, 2);
    f4(3, 4);
    f4(5, 6);
    return 0;
}
```



