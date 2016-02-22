---
layout: post
title: storm contribute流程翻译
description: 先从contribute开始吧
category: blog
---
原文在[这里][1]，简单翻译一下，做个参考，相信用得着。

##开始贡献
[Issue tracker][2]中的某些issue标上了"New bee"标签，如果你对向storm贡献代码感兴趣但是不知从何下手的话，从它们开始吧。由于这些issue仅仅涉及代码库中的某一部分，而且工作量相对较小，因此是入门学习代码库的好方法。

##学习代码库
Wiki中的[实现文档][3]部分描述了代码库的详细概览，想要理解代码库的话，强烈推荐通读这些文档。

##贡献流程
贡献给storm代码库的内容需要以Github pr的形式提交，如果对pr存在疑问的话，我们可以通过Github评论功能迭代改进。

对于小的补丁，仅仅直接提交pr就ok了。对于大的贡献，请遵循以下流程。该流程的目的是为了减少多于工作以及提早发现设计问题：

1. 在[JIRA issue tracker][2]中创建一个issue，如果还不存在的话。
2. 在评论里描述你实现该issue的方案，解释清楚你需要新增哪几块代码，以及他们如何结合。
3. Storm committer会迭代完善你的设计，保证你没有走错方向。
4. 通读[开发者文档][4]里关于如何编译，编码风格，测试等内容。
5. 实现你的issue，提交一个以JIRA ID为前缀的pr（如 `"STORM-123: add new feature foo"`），然后进入（Github的）迭代。

##贡献文档
非常欢迎贡献文档！最好的方式是通过邮件形式向邮件列表发送贡献内容。

[Yaotc]:    http://yaotec.info  "Yaotc"
[1]:   http://storm.apache.org/contribute/Contributing-to-Storm.html "Contributing-to-Storm"
[2]:   https://issues.apache.org/jira/browse/STORM "issue tracker"
[3]:   http://storm.apache.org/documentation/Implementation-docs.html "Implementation-docs"
[4]:   https://github.com/apache/storm/blob/master/DEVELOPER.md "DEVELOPER md"