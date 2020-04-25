---
layout: post
categories: "DataStructure"
title: "Steak And Queue"
author: "TY_K"
---

### Steak

線形資料構造の一種で、Last In First Out (LIFO)。 すなわち、後から入った元素が先に出る。 これはStack の最大の特徴だ。 どんどん積み重なる構造で、先にStackに入るようになった元素は、一番底に敷かれることになる。 そのため、遅く入った要素はその上に蓄積され、呼び出し時に最も上にある要素が呼び出される構造だ。

### Queue

線形資料構造の一種で、First In First Out (FIFO)。 つまり、先に入った要素が先に出てくる。 Stack とは反対に、先に入った要素が一番前で待機してから先に出てくる仕組みである。 ちなみに、Java Collection で Queue はインタフェースである。 これを実装しているPriority Queueなどを使用することができる。

[参照][DataStructure]

[DataStructure]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#stack-and-queue "DataStructure"