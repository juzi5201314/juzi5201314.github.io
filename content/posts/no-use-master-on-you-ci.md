---
title: "最好不要在你的ci里使用其他软件的master分支"
date: 2021-04-25T22:10:37+08:00
draft: false
---

为什么怎么说呢, 因为我觉得, 在ci里使用到其他的仓库, 
就不应该直接拉某个分支的最新commit,
否则别人的提交稍微一影响到你要使用的部分,
很有可能就会出问题.

就在今天, 我发现了[deno-drash]最近的pr和commit的ci全炸了,
test有很多失败掉, 其中我的pr也在其中(没错就是上一篇文章那个).

原因是deno前几天的某一个commit, 更改了Headers的部分逻辑和实现,
会影响[deno-drash]的单元测试, 而deno的ci用的是deno的main分支...
所以... 全部炸了.

那么好的操作应该是怎么样的, 我认为, 应该在本地测试成功之后,
ci固定一个commit, 如果要更新, 就先本地测试通过了之后, 
在更新ci的.

(当然, 集成测试也炸了,不过那有可能是因为另外一个commit,
那边的另外一个开发者在排查和修复)

[deno-drash]: (https://github.com/drashland/deno-drash)
