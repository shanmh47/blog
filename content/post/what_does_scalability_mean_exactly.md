---
title: "What does scalability mean in concurrency, exactly?"
date: 2018-05-11T13:49:32+08:00
draft: false
categories:
- 他山之石，可以攻玉
---

>As a bonus, shrinking the synchronized block may also improve **scalability** as well  

-

>Chapter 11 shows how to improve **scalability** through finner-grained locking strategies  

-

>The problem of unreliable iteration can again be addressed by client-side locking, at some additional cost to **scalability**  

上面几句话中，scalability 是什么意思呢，每一次碰到都有点疑惑不解呢。scalability 中文翻译是：可扩展性。可是可扩展性和并发有什么关系呢？

answer
---

Scalability: It is the ability to accommodate growing number of inputs. If your program is working x number of inputs, will it be able to accomadate a large dataset compared to x inputs . If the design or system fails when a quantity increases, it does not scale.

