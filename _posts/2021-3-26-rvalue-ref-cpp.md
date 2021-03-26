---
layout: post
title: Rvalue references with examples
---

Rvalue references (`T&&`) is a feature for C++ > 11.

## Type deduction with rvalue reference

Consider the following templated function:
```
template<typename T>
void func(T&& x);
```
We want to take a reference to the input argument `x`, but we are not sure about its type. If we use `(T& x)`, then it will fail if we pass a literal, e.g., `func(5)`. If we use `(const T& x)`, then the reference to `x` will be forced to be constant and we can't modify its value inside the function. 

Thus, `(T&& x)` is used for flexibility. `T` in `T&& x` is deduced differently depending on whether `x` is an lvalue or not. 

Specifically, if `x` is an lvalue, then `T` is deduced to be `decltype(x)&`. If `x` is an rvalue, then `T` is deduced to be `decltype(x)`

## Reference collapse

After we know `T`, What would `T&&` then turn into? The rules are defined as follows:
- If `T` is `int`: `T& -> int&, T&& -> int&&`
- If `T` is `int&`: `T& -> int&, T&& -> int&`
- If `T` is `int&&`: `T& -> int&, T&& -> int&&`

## Examples

Now, let us fill the function with the following:
```
void func(T&& x){
    func2(static_cast<T&&>(x));
}
```

### Case 1

```
a = int 5;
func(a);
// Deducing template parameter:
//   a is an lvalue. So T == int&.
//   Call instantiated as func<int&>(int& && a) -> func<int&>(int& a).
// Collapsing reference inside the function:
//   static_cast<T&&> -> static_cast<int& &&> -> static_cast<int&>
//   A reference to x is passed to func2.
```

### Case 2

```
func(5);
// Deducing template parameter:
//   5 is an rvalue. So T == int.
//   Call instantiated as func<int>(int&& a).
// Collapsing reference inside the function:
//   static_cast<T&&> -> static_cast<int&&>
//   An rvalue expression is passed to func2.
```

## References
[1] [This](https://stackoverflow.com/questions/3582001/what-are-the-main-purposes-of-using-stdforward-and-which-problems-it-solves/3582313#3582313) Stack Overflow answer.