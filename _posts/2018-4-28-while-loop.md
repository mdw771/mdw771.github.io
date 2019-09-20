---
layout: post
title: Correctly understanding the role of tf.while_loop()
---

`while_loop()` in Tensorflow is aimed to reduce the number of nodes in the graph. In this sense, it should be viewed as an approach to reducing memory usage, instead of speeding up learning/optimization. In fact, models containing `while_loop` often suffer significant performance drop. This makes `while_loop` not necessarily a better implementation than a static `for` loop. Use `while_loop` mainly in occasions when the script is taking too long to build the graph, or when memory usage is forbiddingly excessive. Nesting of `while_loop` should be avoided whenever possible. 

---

Tensorflow中的`while_loop()`函数的主要作用是减少图中的节点。从这一点来看，我们应当把它视作一种减少内存占用的途径，而非一种提高运算速度的方法。事实上，含有`while_loop()`的脚本性能时常会大打折扣。这一点使`while_loop()`并不一定优于静态的for循环。个人建议主要在两种情况下使用`while_loop()`：一是构建模型花费的时间太长；二是内存使用过高。另外，应当尽一切可能避免`while_loop()`的嵌套。
