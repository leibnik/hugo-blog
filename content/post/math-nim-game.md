+++
mathjax = true
title = "一个叫 “拈” 的博弈游戏"
date = "2017-12-19"
tags = ["game", "math", "algorithm"]
categories = ["math"]
+++

## 前言
最近在公司活动中玩了一个博弈游戏，游戏规则比较简单，但其必胜策略却十分耐人寻味，游戏内容就是有三堆物件，数量分别为 $3, 5, 7$ 个，两位玩家轮流从这三堆物件中取物，每次取物只能在三堆物品中选择其中一堆并至少取走一个，可以直接一堆取完，当所有堆都取完时游戏随即结束并且最后取完的玩家判为输。
这个游戏其实是一个叫 Nim 博弈游戏的一种变种玩法，Nim 游戏通常的玩法如下:

> The normal game is between two players and played with three heaps of any number of objects. The two players alternate taking any number of objects from any single one of the heaps. The goal is to be the last to take an object. [#wiki](https://en.wikipedia.org/wiki/Nim#Game_play_and_illustration)

通常 Nim 游戏的获胜目标是争取成为最后一个取完的玩家，如果反着来，最后一个取完的玩家判为输，则是 Nim 游戏的一种变形，暂且称为 Anti-Nim (部分资料的说法)。

据了解这个游戏源自中国，后由到美洲打工的华人外传，之所以称为 Nim 有个说法称是由粤语的“拈”音译而得，Nim 游戏后来也演化出多个版本的玩法，它的必胜策略直到 20 世纪初才由一个叫 [Charles L. Bouton](https://en.wikipedia.org/wiki/Charles_L._Bouton) 的数学家在他的一篇论文中给出。论文传送门：[Nim, A Game with a Complete Mathematical Theory](http://www.jstor.org/stable/1967631?origin=crossref&seq=1#page_scan_tab_contents)

## Nim Game
### 分析

首先尝试分析 Nim 游戏的规律，以取石子为例，假定 $w$ 为石子的总数 $(w \ge 1)$，这些石子被分为 $n$ 堆$(n \ge 1)$，局面状态以 $(x_1, x_2, \ldots,x_n)$ 进行表示:

1. 当 $n = 1$ 同时 $w = 1$ 时，显然先手胜；
2. 当 $n = 2$ 时，可以分为四种情况进行讨论


	**①** $(1, 1)$ 局面，显然先手败；

	**②** $(1, x)$ 局面，其中 $x > 1$，此时先手只需在 $x$ 中取走 $x - 1$，即变为 $(1, 1)$ 这种“安全局面”即可获胜；

	**③** $(x, x)$ 局面，其中 $x > 1$，此时先手取后变为$(x_1, x)$，后手总是能使局面转变为 $(x_1, x_1)$，若先手不服气，后手总能使局面往 **①** 情况发展，先手必败；

	**④** $(x_1, x_2)$ 局面， 其中 $x_1 > 1$ 且 $x_2 > 1$，同时有 $x_1 \neq x_2$，此时先手必定能将局面转变为 **③** ，此时先手必胜；


	这几种情形似乎能让人联系上异或操作符，先回想下异或操作的规则：

	```
	0 $\oplus$ 0 = 0
	0 $\oplus$ 1 = 1
	1 $\oplus$ 1 = 0

	注： $\oplus$ 为异或的数学符号
	```

	根据该规则我们知道，若 $x_1 \neq x_2$ 时，$x_1 \oplus x_2 \neq 0$，若 $x_1 = x_2$ 时，$x_1 \oplus x_2 = 0$

	当堆数为 $2$，总石子数目为 $w$ ，两堆均为相等的数目的石子时，此时 $x_1 = \frac{w}{2}$, $x_1 \oplus x_1 = 0$，先手取了其中一堆的 $x_1 - x_2$ 个，局面变为 $(x_1, x_2)$，此时 $x_2 \oplus x_1 \neq 0$，在此基础上后手依旧能使局面变为 $(x_2, x_2)$，$x_2 \oplus x_2 = 0$，若先手不服气，双方僵持直至前面说的 $(1, 1)$ 的局面，因此先手必败。

	当堆数为 $2$，总石子数目为 $w$ ，两堆为不相等的数目的石子时，局面状态为 $(x_1, x_2)$ 其中 $x_1 > x_2$，此时 $x_2 \oplus x_1 \neq 0$，先手能使局面变为 $(x_2, x_2)$，$x_2 \oplus x_2 = 0$，游戏进行下去先手必胜。

	针对以上 $n = 2$ 时的两种情形的讨论可以发现，堆数为 $2$ 时，两堆的数目的异或结果是否为 $0$ 直接关系到先手是否能取胜（双方都采取最优策略的前提下），因此可以得到这样的结论：**当 $n = 2$ 时，对于 $(x_1, x_2)$，$x_1 > 0$ 且 $x_2 > 0$ ，利用异或操作符我们可以判断出先手的胜负，此时若 $x_1 \oplus x_2 = 0$ ，则后手存在必胜策略，反之 $x_1 \oplus x_2 \neq 0$ 则先手存在必胜策略。**

3. 当 $n > 2$ 时，情况又变得更为复杂，但以下两种情形还是显而易见的：


	**①** 所有堆均为 $1$ 的情况，此时若 $n$ 为偶数，显然先手败，若 $n$ 为奇数时，先手胜，我们发现此情形下依旧能使用异或进行对先手的胜负进行判断，对于$1 \oplus 1 \oplus \ldots \oplus 1$，当 $1$ 的个数若为偶数则异或结果为 $0$，个数若为奇数时结果不为 $0$，符合 $n = 2$ 时所得出的结论。

	**②** 所有堆中有且只有一个大于 $1$ 的堆，即 $(1, 1, 1, \ldots,x)$，若为 $1$ 的堆数为偶数时，先手将 $x$ 的那堆直接拿完，后手必败，若为 $1$ 的堆数为奇数时，先手仍可从 $x$ 那堆中拿走 $x - 1$ 个，使局面转变为所有的堆均为 $1$ 且堆数为偶数，此情形下先手必胜。同理的，$1 \oplus 1 \oplus \ldots \oplus x$ 在 $x > 1$ 时，值始终不为 $0$，同样可以以此判断先手的胜负。


对于其他情况已经难以分析了，但也能隐约看出异或运算就是关键，难点就在于如何证明这个判定方法对于所有情形通用。


### 组合博弈（Combinatorial Game）
符合以下特性的博弈称为组合博弈：

```
1. 只有两位玩家
2. 游戏的局面是有限集
3. 游戏规则针对双方，每个局面的转移都要合法。如果两者的移动规则一样，称为对等博弈(impartial games)，否则为不对等博弈(partizan games)。
4. 双方轮流操作
5. 当轮到一方已无法继续操作时，游戏结束。在正常的游戏规则下，最后一个操作的人赢。在 misere 游戏规则下，最后一个操作的人输。
6. 游戏在有限步结束。
```
Nim Game 显然符合组合博弈的定义，由于移动规则对先后手一样，所以更为准确的说是对等组合博弈 (ICG: Impartial Combinationrial Game)。Anti-Nim Game 同样属于 ICG，只不过是采用了 [misere](https://en.wikipedia.org/wiki/Mis%C3%A8re#Mis%C3%A8re_game) 规则的 Nim Game。

### P态(P-position)、N态(N-position)
P 即 Previous，N 即 Next。规定局面对于上一个操作者可赢的局面状态为 P(Previous) 态，对于接下来的操作者可赢的局面状态为 N(Next) 态，而当局面操作无法继续进行时则定义为终态(Terminal Position)。

对于正常规则下的 ICG，P 态和 N 态有以下几个特征：

```
1. 所有的终态都是 P 态。
2. 从 N 态开始总有方法转化为 P 态。
3. 从 P 态开始总是无法避免的会转化为 N 态。
```

对于采用 misere 规则的 ICG，区别只在于所有的终态都是 N 态。也有一些资料以是否必败来区分局面状态的，但都与前面的 P 态、N 态 性质一致，定义如下：


在对方使用最优策略时，无论做出什么决策都会导致失败的局面称为必败态，其他的局面则称为非必败态。必败态和非必败态有以下性质：

```
1. 若面临末状态者为获胜则末状态为非必败态否则末状态为必败态。
2. 一个局面是非必败态的充要条件是该局面进行某种决策后可以成为必败态。
3. 一个局面是必败态的充要条件是该局面无论进行何种决策均会成为非必败态。
```

### Bouton 定理及证明

要证明一种判定 position 性质的方法的正确性需要证明以下三个命题：

```
1. 这个判定方法判定终态(Terminal position)为 P-position（在 misere rule 下则为 N-position）。
2. 被该方法判定为 P-position 的局面，无法转化为其他 P-position 的局面，或者说必定会转化为 N-position。
3. 被该方法判定为 N-position 的局面，至少存在一种往 P-position 转化的途径。
```

**定理： 对于一个 Nim 游戏，局面为 $(x_1,\ldots, x_n)$，它是 P-position 当且仅当 nim-sum $= 0$ （即: $x_1 \oplus \ldots \oplus x_n = 0$），相对的，当 nim-sum $\neq 0$ 时，局面为 N-position。**

```
证明：

Nim Game 的终态为(0,...,0)，$0$ $\oplus$...$\oplus$ $0$ = 0，因此终态为 P-position，命题 1 得证。

注意到 nim-sum 的运算满足类似加法的结合律与交换律，同时还有两个相同的数异或结果为 0 的特性。我们以 $x_1$,..., $x_n$ 表示移动前的状态，以 $y_1$,...,$y_n$ 为移动后的状态，设 s = $x_1$ $\oplus$...$\oplus$$x_n$ ，t = $y_1$ $\oplus$...$\oplus$$y_n$，当移动的堆为第 k 堆，我们有 $x_i$ = $y_i$ 且 i ≠ k，$x_k$ > $y_k$。根据异或的特性可得：

t = 0 $\oplus$ t
  = s $\oplus$ s $\oplus$ t
  = s $\oplus$ ($x_1$ $\oplus$ ... $\oplus$ $x_n$) $\oplus$ ($y_1$ $\oplus$ ... $\oplus$ $y_n$)
  = s $\oplus$ ($x_1$ $\oplus$ $y_1$) $\oplus$ ... $\oplus$ ($x_n$ $\oplus$ $y_n$)
  = s $\oplus$ 0 $\oplus$ ... $\oplus$ 0 $\oplus$ ($x_k$ $\oplus$ $y_k$) $\oplus$ 0 $\oplus$ ... $\oplus$ 0
  = s $\oplus$ $x_k$ $\oplus$ $y_k$

因此： t = s $\oplus$ $x_k$ $\oplus$ $y_k$

对于 P-position 的局面，有 s = 0，t = $x_k$ $\oplus$ $y_k$ 又由异或的特性可得 $x_k$ $\oplus$ $y_k$ ≠ 0，因此 t ≠ 0 。即 P-position 总是无法避免的会转化为 N-position，命题 2 得证。

对于 N-position 的局面，有 s ≠ 0，设第 d 位为 s (以二进制表示) 最左边不为 0 的位，可以找到第 k 堆 $x_k$ (以二进制表示) 的第 d 位不为 0 。再使 $y_k$ = s $\oplus$ $x_k$，已知 $x_k$ > $y_k$ ，$x_k$ 与 $y_k$ 的二进制位从最左边至 d 位相同。将 d 位的 1 变为 0 ，d 位右侧余下的二进制位最高为 $2^d$ - 1。第一位玩家可以从第 k 堆中拿走 $x_k$ - $y_k$ 的物品，使得

t = s $\oplus$ $x_k$ $\oplus$ $y_k$
  = s $\oplus$ $x_k$ $\oplus$ (s $\oplus$ $x_k$)
  = 0

即 N-position 总能找到方法使得局面向 P-position 转化，命题 3 得证。

证毕。
```

针对实际进行的 Nim 游戏，当玩家面对 $x_1 \oplus x_2 \oplus \ldots \oplus x_k \ldots \oplus x_n = 0$ 的局面时，无论怎么取都会使 $x_1 \oplus x_2 \oplus \ldots \oplus y_k \ldots \oplus x_n \neq 0$，而当玩家面对 $x_1 \oplus x_2 \oplus \ldots \oplus x_k \ldots \oplus x_n \neq 0$ 则总有办法使 $x_1 \oplus x_2 \oplus \ldots \oplus y_k \ldots \oplus x_n = 0$。因此对于 Nim 游戏有结论：**当 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$  则后手存在必胜策略，否则先手存在必胜策略，当 $x_1 \oplus x_2 \oplus \ldots \oplus x_n \neq 0$ 时，先手若采取最优策略必胜，此时的必胜策略也是显而易见的，只要每次使 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$ 把必败态交给对方即可取胜。**

## Anti-Nim Game

Nim 游戏是最后取完的人获胜，而 Anti-Nim 则是最后取完的人输。

有了前面对 nim 游戏的分析和证明，我们直接分析 anti-nim 游戏 $n$ 堆 $(n > 0)$ 的情形:

1. 当 $n$ 堆石子所有堆的石子的个数均为 $1$，即 $(1, 1, \ldots, 1)$：

	① 当 $n$ 为偶数时，为先手的非必败态；

	② 当 $n$ 为奇数时，为先手的必败态；

2. 当 $n$ 堆石子中有且仅有一个石子数目大于 $1$ 的堆，即 $(1, \ldots, x, \ldots,1)$：

	① 当 $n - 1$ 为偶数时，先手可将石子数目大于 $1$ 的堆的石子取剩至 $1$ ，转变为 1.②，先手胜；

	② 当 $n - 1$ 为奇数时，先手直接将石子数目大于 $1$ 的那堆取完，1.②，先手胜；

	因此此场景为先手的非必败态。

3. 当 $n$ 堆石子中存在两个以上石子数目大于 $1$ 的堆，局面为 $(x_1, x_2,\ldots, x_n)$：

	① 当 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$ 时，只可能向两种状态转变

	  	(a). n 堆石子中有且仅有一个石子数目大于 1 的堆；

	  	(b). 依旧存在两个以上石子数目大于 1 的堆，但 $x_1$ $\oplus$ $x_2$ $\oplus$ ... $\oplus$ $x_n$ ≠ 0；

	  此时(a), (b) 两种状态对后手而言是非必败态，也就是说 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$ 时，后手存在必胜策略。

	② 当 $x_1 \oplus x_2 \oplus \ldots \oplus x_n \neq 0$ 时，先手总有办法使局面转化为 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$，此时对先手而言，$x_1 \oplus x_2 \oplus \ldots \oplus x_n \neq 0$ 为非必败态，而后手遇到 3.① 的局面只能又把非必败态交给对方。

结合以上几种情形可以得出结论: **当所有堆的石子数均为 $1$ 且 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$ 或 至少存在一个以上石子数目大于 1 的堆且 $x_1 \oplus x_2 \oplus \ldots \oplus x_n \neq 0$时，先手存在必胜策略，否则后手存在必胜策略。**

以 $(3, 5, 7)$ 为例，符合至少存在一个以上石子数目大于 $1$ 的堆且 $x_1 \oplus x_2 \oplus \ldots \oplus x_n \neq 0$，此时先手存在必胜策略。

 |$2^2$|$2^1$|$2^0$
:---|:---|:---|:---
3|0|1|1
5|1|0|1
7|1|1|1
nim_sum|0|0|1

必胜策略能以异或运算求得，先求出 nim_sum，在这个例子里 nim_sum = 1；再将 nim_sum 与各个堆的数目进行异或，结果小于原来堆的数目即是合理的策略。

```
nim_sum $\oplus$ 3 = 2; 2 < 3 合理的策略，可直接在此堆中取走 1 个，使得 nim_sum = 0
nim_sum $\oplus$ 5 = 4; 4 < 5 同理
nim_sum $\oplus$ 7 = 6; 6 < 7 同理
```

而要注意的是，当局面转变为有且仅有一个石子数目大于 1 的堆，即 $(1, \ldots, x, \ldots,1)$时，策略就不再是使 nim_sum $= 0$，而是使所有堆的石子的个数均为 $1$ 且堆数 $n$ 为奇数，确保了再次轮到自己时所有堆的石子数均为 $1$ 且 $x_1 \oplus x_2 \oplus \ldots \oplus x_n = 0$。

## 刷题
搜集了几道有关 Nim Game 的编程题：

[杭电oj：取(m堆)石子游戏](http://acm.hdu.edu.cn/showproblem.php?pid=2176)

[LeetCode: Nim Game](https://leetcode.com/problems/nim-game/)

[codewars 4ky 题目: Nim](https://www.codewars.com/kata/nim/train/javascript)

利用已知的结论来求解问题并不困难，以这道 [codewars 4ky 题目: Nim](https://www.codewars.com/kata/nim/train/javascript) 为例：

>This kata explores writing an AI for a two player, turn based game called NIM.

> **The Board**
>
> The board starts out with several piles of straw. Each pile has a random number of straws.
>
> Pile 0: ||||
>
> Pile 1: ||
>
> Pile 2: |||||
>
> Pile 3: |
>
> Pile 4: ||||||
>
> ...or more concisely: [4,2,5,1,6]
>
> **The Rules**
>
> The players take turns picking a pile, and removing any number of straws from the pile they pick
> A player must pick at least one straw
> If a player picks the last straw, she wins!
>
> **The Task**
>
> In this kata, you have to write an AI to play the straw picking game.
>
> You have to encode an AI in a function choose_move (or chooseMove, or choose-move) that takes a board, represented as a list of positive integers, and returns
>
> *[pile_index, number_of_straws]*
>
> Which refers to an index of a pile on the board, and some none-zero number of straws to draw from that pile.
>
> The test suite is written so that your AI is expected to play 50 games and win every game it plays.

题目大意就是给出一个以数组进行表示的局面状态，解题者作为先手需要下出必胜着，以 *[pile_index, number_of_straws]* 形式返回，即堆的序号以及需取走的数目。这里需要应用到定理中得出的公式: `t = s $\oplus$ $x_k$ $\oplus$ $y_k$`，依据题意，我们需要做的就是使得 t = 0，再求出 `$y_k$`，即变化后的数目，最后`$x_k$ - $y_k$`就是我们需取走的数目，于是有：

```
0 = s $\oplus$ $x_k$ $\oplus$ $y_k$
s $\oplus$ 0 = s $\oplus$ s $\oplus$ $x_k$ $\oplus$ $y_k$
s = $x_k$ $\oplus$ $y_k$
$y_k$ = s $\oplus$ $x_k$
```

翻译成代码就是如下：

```javascript
function chooseMove(state) {
  let nim_sum = state.reduce((s, a) => s ^= a, 0);
  let n = state.find((element, index, arr) => (element ^ nim_sum) <= arr[index]);
  return [state.indexOf(n), n - (n ^ nim_sum)];
}
```

## 参考
[组合数学](https://book.douban.com/subject/10606626/) 1.7 节

[编程之美](https://book.douban.com/subject/3004255/)	1.12 节

[Nim, A Game with a Complete Mathematical Theory](http://www.jstor.org/stable/1967631?origin=crossref&seq=1#page_scan_tab_contents)

[wiki#Nim](https://en.wikipedia.org/wiki/Nim#Game_play_and_illustration)

[Game Theory](https://www.cs.cmu.edu/afs/cs/academic/class/15859-f01/www/notes/comb.pdf)
