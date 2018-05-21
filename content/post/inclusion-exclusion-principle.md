+++
title = "容斥原理（一）"
date = "2018-05-20"
mathjax = true
tags = ["math", "algorithm"]
categories = ["math"]
+++
### quiz
```
不超过 999 的正整数中，3 或 5 的倍数的和是多少？
```

### solve
效率低的做法就是依次枚举累加 3 或 5 的倍数来求解。

```javascript
[...Array(1000).keys()].reduce((sum,i) => i % 3 === 0 || i % 5 === 0 ? sum + i : sum, 0);
```
处理这类问题，组合数学中有非常重要的一个计数公式——容斥原理（Inclusion–exclusion principle）。

设 $S$ 是对象的有限集合，且 $P_1$ 和 $P_2$ 是每一个对象有或没有的两个性质。设 $A_1$ 是 $S$ 中具有 $P_1$ 性质的对象的集合，$A_2$ 是 $S$ 中具有 $P_2$ 性质的对象的集合。在计数 $S$ 中既不具有性质 $P_1$ 也不具有性质 $P_2$ 的对象的个数时，我们可以从集合 $S$ 中排除掉集合 $A_1$ 和集合 $A_2$，但由于重复排除了兼具有 $P_1$ 和 $P_2$ 性质的对象，所以还需补充上集合 $A_1$ 与 $A_2$ 相交的部分。于是有:

$$|\overline{A}_1 \cap \overline{A}_2| = |S| - |A_1| - |A_2| + |A_1 \cap A_2|$$

继续推广至求解集合中不具备 m 个性质的对象的个数，


集合 $S$ 中不具有性质 $P_1$, $P_2$, ..., $P_m$ 的对象个数由下面的交错表达式给出：

$$
|\overline{A}_1 \cap \overline{A}_2 \cap \dots \cap \overline{A}_m|=
$$
$$
|S| - \sum|A_i| + \sum|A_i \cap A_j| - \sum|A_i \cap A_j \cap A_k|+\dots+(-1)^m|A_1 \cap A_2 \cap \dots \cap A_m|
$$

其中，第一个和对 $\\{1, 2, \dots, m\\}$ 的所有1子集 $\\{i\\}$ 求和，第二个和对 $\\{1, 2, \dots, m\\}$ 的所有 2 子集 $\\{i, j\\}$ 求和， 第三个和对 $\\{1,2, \dots, m\\}$ 的所有 3 子集 $\\{i, j, k\\}$ 求和，一直进行下去，直到第 $m$ 个和是对 $\\{1, 2, \dots, m\\}$ 的所有 $m$ 子集求和，这个 $m$ 子集只有一个，就是原集合本身。

由德摩根公式，可得推论：
$$
|A_1 \cup A_2 \cup \dots \cup A_m| 
= |S| - |\overline{A}_1 \cap \overline{A}_2 \cap \dots \cap \overline{A}_m|=
$$

$$ 
\sum|A_i| - \sum|A_i \cap A_j| + \sum|A_i \cap A_j \cap A_k|-\dots+(-1)^{m+1}|A_1 \cap A_2 \cap \dots \cap A_m|
$$

根据以上推论可以求出任意集合中符合至少一个限定的性质的对象的个数。

回到最初的问题，明显的，被 3 整除和被 5 整除是每个数有或没有的两个性质，我们可以直接得到不超过 999 的正整数中能被 3 整除的个数和能被 5 整除的个数，而能同时被 3 和 5 整除的数也即是能被 15（3 和 5 的最小公倍数 ）整除。再由等差数列求和公式分别求和，根据推论求得结果。

```javascript
const sn = n => n * (n + 1) / 2;

function solution(number){
  let n3 = (number - 1) / 3 | 0;
  let n5 = (number - 1) / 5 | 0;
  let n15 = (number - 1) / 15 | 0;
  return sn(n3) * 3 + sn(n5) * 5 - sn(n15) * 15;
}

```