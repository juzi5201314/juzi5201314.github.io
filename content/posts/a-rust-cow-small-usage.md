---
title: "一个rust Cow的应用场景"
date: 2021-04-25T21:48:12+08:00
draft: false
---
> [Cow]: Clone on Write(写时克隆)

昨天我在给[deno-drash]提的pr提交代码, 发现ci跑unit test失败了, 于是在找错误. 

找到deno某个commit的时候, 看到别人的代码, 其中一段用了Cow. 我之前一直没明白Cow平时能在哪用上, 看到这段代码之后突然就懂了其中一个使用场景.


在这样一个场景里: 
> match有3个分支, 其中两个返回&str, 一个返回String
```rust
let var: ? = match() {
A => str1,
B => str2,
C => string3,
}
```
在以前,我会这么做:
```rust
let var: String = ..
...
A => str1.to_string(),
B => str2.to_string(),
C => string3,
```

这么写的话, A和B的&str转为String的时候, 就必然要复制一次.

但是如果用Cow的话:
```rust
let var: Cow<str> = ..
...
A => Cow::Borrowed(str1),
B => Cow::Borrowed(str2),
C => Cow::Owned(string3),
```
这样子, 只要你后面不更改var的值, 就不会发生内存复制.

技巧+1🤪

[deno-drash]: (https://github.com/drashland/deno-drash)
[Cow]: (https://doc.rust-lang.org/std/borrow/enum.Cow.html)