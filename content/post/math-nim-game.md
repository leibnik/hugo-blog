+++
mathjax = true
title = "走进科学之 3,5,7 取物游戏的背后"
date = "2017-12-19"
tags = ["game", "math", "algorithm"]
categories = ["math"]
+++

### 简述
3，5，7 取物游戏其实是一个叫 Nim 博弈游戏的一种变种玩法，Nim 游戏通常的玩法如下:

> The normal game is between two players and played with three heaps of any number of objects. The two players alternate taking any number of objects from any single one of the heaps. The goal is to be the last to take an object. [wiki](https://en.wikipedia.org/wiki/Nim#Game_play_and_illustration)

通常 Nim 游戏的获胜目标是最后一个取完，如果反着来，最后一个取完的玩家判为输，就是 Nim 游戏的变形，叫 anti-nim

先来分析下 Nim 游戏的规律(注意是先取完为胜的玩法)，以取石子为例，假定 w 为石子的总数(w >= 1)，这些石子被分为 n 堆(n >= 1)，局面状态以 ( $w_1$ , $w_2$, ...,$w_n$ ) 进行表示:

1. 当 n = 1 同时 w = 1 时，显然先取者胜
2. 当 n = 2 时，可以分为四种情况进行讨论

	
	**①** (1, 1) 状态，显然先手败

	**②** (1, x) 状态，其中 x > 1，此时先手只需在 x 中取走 x - 1，即变为 (1, 1) 这种“安全局面”即可获胜

	**③** (x, x) 状态，其中 x > 1，此时先手取后变为( $x_1$ , x)，后手总是能使局面转变为 ($x_1$, $x_1$)，若先手不服气，后手总能使局面往 **①** 情况发展，先手必败

	**④** ($x_1$, $x_2$) 状态， 其中 $x_1$ > 1 且 $x_2$ > 1，同时有 $x_1$ 不等于 $x_2$，此时先手必定能将局面转变为 **③** ，此时先手必胜
	

	先回想下异或操作的规则：

	```
	XOR(0, 0) = 0
	XOR(0, 1) = 1
	XOR(1, 1) = 0
	```

	根据该规则我们知道，若 $x_1$ ≠ $x_2$ 时，XOR($x_1$, $x_2$) ≠ 0，若 $x_1$ = $x_2$ 时，XOR($x_1$, $x_2$) = 0
	
	当堆数为 2，总石子数目 w 为偶数时，两堆均为相等的数目的石子，此时 $x_1$ = w / 2, XOR($x_1$, $x_1$) 等于 0，先手无论怎么取都将使 XOR($x_2$, $x_1$) 不等于 0，在此基础上后手依旧能使局面变为 ($x_2$, $x_2$)，XOR($x_2$, $x_2$) 等于 0，若先手不服气，双方僵持直至前面说的 (1, 1) 的局面，因此先手必败

	当堆数为 2，总石子数目 w 为奇数时，此时与 w 为偶数状态时相反，此时异或规则依旧适用于判断先手的胜负

	经过上述 n 为 2 情况的讨论，我们可以得到这样的结论，**n = 2 时，对于 ($x_1$, $x_2$)，$x_1$ > 0 且 $x_2$ > 0 ，利用异或操作符我们可以判断出先手的胜负**

3. 当 n > 2 时，情况又变得更为复杂，不妨再分出以下几种情形

	
	**①** 所有堆均为 1 的情况，此时若 n 为偶数，显然先手败，若 n 为奇数时，先手胜
	
	**②** 所有堆中有且只有一个大于 1 的堆，即 (1, 1, 1,...,x)，若为 1 的堆数为偶数时，先手将 x 的那堆直接拿完，后手必败，若为 1 的堆数为奇数时，先手仍可从 x 那堆中拿走 x - 1 个，使局面转变为所有的堆均为 1 且堆数为偶数，此情形下先手必胜
	
	**③** 

`将所有堆的石子数进行异或(xor)，值为 0 则先手输，否则先手胜`

## 参考
[组合数学](https://book.douban.com/subject/10606626/) 1.7 节

[编程之美](https://book.douban.com/subject/3004255/)	1.12 节

## 值得一刷的题
[杭电oj：取(m堆)石子游戏](http://acm.hdu.edu.cn/showproblem.php?pid=2176)

[LeetCode: Nim Game](https://leetcode.com/problems/nim-game/)

[codewars 4ky 题目: Nim](https://www.codewars.com/kata/nim/train/javascript)
