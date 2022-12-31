---
layout: default
---

# 2022全國資訊學科能力競賽 解說（NHSPC2022 Editorial）

---

## A - base

---

## B - bicycle

最小權最大二分匹配：

這題可以看成是腳踏車與空位之間的最小權最大二分匹配，可以建出一張完全二分圖 K<sub>n,m</sub>，邊權即為移動的距離。

可以用 costflow 或者匈牙利算法解，複雜度可以做到 O((n+m)<sup>3</sup>)。

----

交換率：

設 b<sub>1</sub>, b<sub>2</sub> 為兩台腳踏車座標， s<sub>1</sub>, s<sub>2</sub> 為兩個空位座標。
且 b<sub>1</sub> < b<sub>2</sub> < s<sub>1</sub> < s<sub>2</sub>。

可以發現移動方式 b<sub>1</sub>s<sub>1</sub> b<sub>2</sub>s<sub>2</sub> 與 b<sub>1</sub>s<sub>2</sub> b<sub>2</sub>s<sub>1</sub>
的移動總距離是一樣的。

```graphviz
digraph {
  b1
  b2
  s1 [style=filled];
  s2 [style=filled];
  b1 -> b2 -> s1 -> s2 [style="invis"];
  
  {rank=min; b1 -> s1;}
  {rank=min; b2 -> s2;}
}
```

```graphviz
digraph {
  b1
  b2
  s1 [style=filled];
  s2 [style=filled];
  b1 -> b2 -> s1 -> s2 [style="invis"];
  
  {rank=min; b1 -> s2;}
  {rank=min; b2 -> s1;}
}
```

(b=腳踏車, s=空位)

----

O(nm) DP：

根據上面的交換率，必定存在一組最佳解，使得**座標越大的腳踏車移動到的空位座標也越大**。

我們可以記錄 `dp[i][j]` = 從左到右前 i 台腳踏車都歸還，最後一台腳踏車歸還在從左到右第 j 空位的最小花費。
其中轉移可以用紀錄前綴最小值的方式均攤 O(1)，
並且 DP 表格可以只記錄兩列因此空間複雜度可以省略到 O(m)。

```graphviz
digraph {
  s1 [style=filled];
  s2 [style=filled];
  b1;
  b2;
  s3 [style=filled];
  s4 [style=filled];
  b3;
  b4;
  s5 [style=filled];
  s6 [style=filled];
  {rank=min; s1 -> s2 -> b1 -> b2 -> s3 -> s4 -> b3 -> b4 -> s5 -> s6 [style="invis"]};
  
  b1 -> s2;
  b2 -> s3;
  b3 -> s5;
  b4 -> s6;
}
```
(腳踏車位置越大，移動到的位置越大)

----

O(m log m) DP：

**性質1**： 根據上面的交換率，必定存在一組最佳解，使得每台腳踏車以及其歸還位置的連線線段**不存在部分相交**，
也就是說要嘛一線段完全包含另一線段，或者完全不相交。
（例如 b<sub>1</sub> < b<sub>2</sub> < s<sub>1</sub> < s<sub>2</sub>， b<sub>1</sub>s<sub>1</sub> b<sub>2</sub>s<sub>2</sub> 
這個還法有部分相交，但可以換成 b<sub>1</sub>s<sub>2</sub> b<sub>2</sub>s<sub>1</sub>）

```graphviz
digraph {
  s1 [style=filled];
  s2 [style=filled];
  b1;
  b2;
  s3 [style=filled];
  s4 [style=filled];
  b3;
  b4;
  s5 [style=filled];
  s6 [style=filled];
  {rank=min; s1 -> s2 -> b1 -> b2 -> s3 -> s4 -> b3 -> b4 -> s5 -> s6 [style="invis"]};
  
  b1 -> s2;
  b2 -> s3;
  b3 -> s6;
  b4 -> s5;
}
```
(一個不存在部分相交的例子)

**性質2**： 若有一**不存在部分相交**的最佳解，在這個解裡腳踏車 b 還車到空位 s，則 b,s 之間所有空位一定都有停還來的腳踏車 ⇛
很容易反證，如果 b, s 間存在一個空位沒還車，則我們可以把 b,s 間的所有腳踏車還的位置全部往空位的方向靠，可以構造出一組移動距離更小的解。

```graphviz
digraph {
  b1;
  b2;
  s3 [style=filled];
  s4 [style=filled];
  s5 [style=filled];
  {rank=min; b1 -> b2 -> s3 -> s4 -> s5 [style="invis"]};
  
  b1 -> s5;
  b2 -> s4;
}
```
(b1, s5 中間有一個空位未使用)

```graphviz
digraph {
  b1;
  b2;
  s3 [style=filled];
  s4 [style=filled];
  s5 [style=filled];
  {rank=min; b1 -> b2 -> s3 -> s4 -> s5 [style="invis"]};
  
  b1 -> s4;
  b2 -> s3;
}
```
(b1, b2歸還點都往左移填滿空格，可以構造出一個更好的解)

**算法**：

根據以上兩個性質，可以發現一輛腳踏車 b 只會有兩個可能被歸還的地方：

* l<sub>b</sub>: 在 b 左邊，滿足 [l<sub>b</sub>, b] 間腳踏車與空格數量相等，座標最大的空位
* r<sub>b</sub>: 在 b 右邊，滿足 [b, r<sub>b</sub>] 間腳踏車與空格數量相等，座標最小的空位

對於每輛腳踏車所對應到的 l<sub>b</sub>, r<sub>b</sub>，可以用類似括號匹配的方法應用 stack 在線性時間求出。
求出所有連線 [l<sub>b</sub>, b], [b, r<sub>b</sub>] 之後，可以再做一次 DP，
紀錄每一個前綴連線匹配完所需要花的總距離，轉移會需要查詢最大值的資料結構做輔助，複雜度為 O(m log m)。

---

## C - card

O(2<sup>n</sup>) 做法：

每一回合可以枚舉要選擇最左邊還最右邊的卡片，全枚舉的話因為一共有 n 回合所以複雜度為 O(2<sup>n</sup>)。

----

O(n<sup>2</sup>) 做法：

可以用 DP 來記錄可得到的最大分數，例如狀態 `dp[x][y]` = [x, y] 選完區間的卡片能得的最大分數，則我們有

- 初始狀態：`dp[x][x] = cost(x, n)`
- 轉移： `dp[x][y] = max{dp[x][y-1] + cost(y, n-y+x), dp[x+1][y] + cost(x, n-y+x)}`

其中 cost(a, b) 是指卡片 a 在第 b 回合選的得分，轉移為 O(1)。

---

## D - editor

---

## E - gears

這題的齒輪方向以及轉速是可以分開討論的:

* 旋轉方向：由左至右，每遇到一個「接合」方向乘以 -1，否則不改變方向
* 旋轉速度：若第 $i$, $i+1$ 個齒輪為接合（$c_i = 1$），轉速除以 $i$ 的齒數乘以 $i+1$ 的齒數，否則轉速不變

複雜度是 O(n)。

---

## F - scratchcard

---

## G - tree

### 簡化問題

若一棵樹 $T$ 的節點 $i$ 的度數為 $d_i$，則 $i$ 在 $T$ 的 Prüfer 序列裡會恰出現 $d_i-1$ 次。反過來說，對任意序列 $\mathbf{p} = p_1, p_2, \ldots, p_{n-2}$，其中 $p_i = 1, 2, \ldots, n$，我們都能找到對應的樹 $T(\mathbf{p})$ 使得 $T(\mathbf{p})$ 的 Prüfer 序列為 $\mathbf{p}$。

要把一個 Prüfer 序列還原成一棵樹，首先我們必須算出這棵樹的度數序列：

> $n \gets |\mathbf{p}|+2$<br/>
> $\mathbf{d} \gets \underbrace{1, 1, \ldots, 1}_{n\text{ copies}}$<br/>
> **for** $p_i$ **in** $\mathbf{p}$ **do**<br/>
> &nbsp;&nbsp;𝑑<sub>𝑖</sub> $\gets d_i+1$<br/>
> **end** **for**

有了度數序列後，就能仿照 Prüfer 序列的生成步驟，逐步把邊加上去：

> $T \gets n$ isolated vertices<br/>
> **for** $p_i$ **in** $\mathbf{p}$ **do**<br/>
> &nbsp;&nbsp;𝑢 $\gets$ the smallest index $i$ satisfying $d_i = 1$<br/>
> &nbsp;&nbsp;Add edge $up_i$ to $T$<br/>
> &nbsp;&nbsp;𝑑<sub>𝑝<sub>𝑖</sub></sub> $\gets d_{p_i}-1, d_u \gets d_u-1$<br/>
> **end** **for**<br/>
> $u, v \gets$ the remaining $2$ indices $i$ satisfying $d_i = 1$<br/>
> Add edge $uv$ to $T$

執行這份虛擬碼後，𝑇 的 Prüfer 序列即為 $\mathbf{p}$。因此本題能簡化成這樣：

> 給定一個序列 $d_1, d_2, \ldots, d_n$，請求出 $i$ 恰出現 $d_i-1$ 次的字典序第 $k$ 小序列。

為了方便，以下假定我們想求 $i$ 恰出現 $n_i$ 次的第 $k$ 小序列，其中 $1 \le i \le m, n_1+n_2+\ldots+n_m = n-2$，且每個 $i$ 均有 $n_i > 0$。

### $O(n^2)$ 演算法

首先觀察滿足條件的序列個數為

$$K := \frac{(n_1+n_2+\ldots+n_m)!}{n_1! n_2! \ldots n_m!}.$$

如果輸入的 $k$ 大於 $K$，直接輸出 $-1$；否則，觀察以 $i$ 為開頭的序列個數為

$$K_i := \frac{(n_1+\ldots+n_m-1)!}{n_1! \ldots (n_i-1)! \ldots n_m!} = K\frac{n_i}{n_1+\ldots+n_m}.$$

計算 $K_1$ 需要 $O(n)$ 時間，接著由於

$$K_i = K_1\frac{n_i}{n_1},$$

可以用 $O(m)$ 時間算出其他的 $K_i$。總共有 $O(n)$ 格要填，故時間複雜度為 $O(n^2)$。

### 更快的演算法

雖然不是本題的考點，由於 $k$ 最大只到 $10^9$，相較 $(n-2)!$ 不大，我們可以利用 $k$ 來得到一個接近線性的演算法。

首先，若 $m \ge 14$，則 $K_1 \ge 13! > 10^9$，故此時可以直接填 $1$；另一方面，若 $m \le 13$，為了在 $O(m)$ 時間內算出 $K_1$，我們必須在 $O(1)$ 時間內算出

$$\frac{(n_1+\ldots+n_{l-1})\times(n_1+\ldots+n_{l-1}+1)\times\cdots\times(n_1+\ldots+n_{l-1}+n_l-1)}{n_l!} = \binom{n_1+\ldots+n_l-1}{n_l}.$$

因此需要先算好所有在 $10^9$ 內的 $\binom{r}{s}$。由於 $\binom{r}{s} = \binom{r}{r-s}$，以下只考慮 $2s \le r$ 的情況。當 $s \le 2$ 時，可以直接回傳計算結果；當 $s \ge 3$ 時，可以在 compile time 建表計算完成：

```cpp
constexpr int K = 1'000'000'000;
constexpr int R = 2000; // any integer n satisfying binom(n, 3) > K
constexpr int S = 20; // any integer n satisfying binom(2n, n) > K
constexpr std::array<std::array<int, S>, R> get_binom(){
  std::array<std::array<int, S>, R> res{};
  res[0][0] = 1;
  for(int i=1; i<R; ++i){
    res[i][0] = 1;
    for(int j=1; j<S; ++j){
      res[i][j] = std::min(res[i-1][j-1]+res[i-1][j], K+1);
    }
  }
  return res;
}
constexpr std::array<std::array<int, S>, R> Binom = get_binom();
```

因此字典序第 $k$ 小序列可以在 $O(\mu n)$ 時間得到，這裡 $\mu$ 是滿足 $\mu! \ge k$ 的任意整數，以本題來說可以取 $\mu = 13$。

---

## H - ussr

---

## I - xmas
