---
layout: default
---

# 2022全國資訊學科能力競賽 解說（NHSPC2022 Editorial）

---

## A - base

---

## B - bicycle

---

## C - card

---

## D - editor

---

## E - gears

---

## F - scratchcard

---

## G - tree

### 簡化問題

若一棵樹 $T$ 的節點 $i$ 的度數為 $d_i$，則 $i$ 在 $T$ 的 Prüfer 序列裡會恰出現 $d_i-1$ 次。反過來說，對任意序列 $\mathbf{p} = p_1, p_2, \ldots, p_{n-2}$，其中 $p_i = 1, 2, \ldots, n$，我們都能找到對應的樹 $T(\mathbf{p})$ 使得 $T(\mathbf{p})$ 的 Prüfer 序列為 $\mathbf{p}$。

要把一個 Prüfer 序列還原成一棵樹，首先我們必須算出這棵樹的度數序列：

> $n \gets |\mathbf{p}|+2$
> $\mathbf{d} \gets \underbrace{1, 1, \ldots, 1}_{n\text{ copies}}$
> **for** $p_i$ **in** $\mathbf{p}$ **do**
> &nbsp;&nbsp;$d_i \gets d_i+1$
> **end** **for**

有了度數序列後，就能仿照 Prüfer 序列的生成步驟，逐步把邊加上去：

> $T \gets n$ isolated vertices
> **for** $p_i$ **in** $\mathbf{p}$ **do**
> &nbsp;&nbsp;$u \gets$ the smallest index $i$ satisfying $d_i = 1$
> &nbsp;&nbsp;Add edge $up_i$ to $T$
> &nbsp;&nbsp;$d_{p_i} \gets d_{p_i}-1, d_u \gets d_u-1$
> **end** **for**
> $u, v \gets$ the remaining $2$ indices $i$ satisfying $d_i = 1$
> Add edge $uv$ to $T$

執行這份虛擬碼後，$T$ 的 Prüfer 序列即為 $\mathbf{p}$。因此本題能簡化成這樣：

> 給定一個序列 $d_1, d_2, \ldots, d_n$，請求出 $i$ 恰出現 $d_i-1$ 次的字典序第 $k$ 小序列。

為了方便，以下假定我們想求 $i$ 恰出現 $n_i$ 次的第 $k$ 小序列，其中 $1 \le i \le m, n_1+n_2+\ldots+n_m = n-2$，且每個 $i$ 均有 $n_i > 0$。

### $O(n^2)$ 演算法

首先觀察滿足條件的序列個數為

$$K := \frac{(n_1+n_2+\ldots+n_m)!}{n_1! n_2! \ldots n_m!}.$

如果輸入的 $k$ 大於 $K$，直接輸出 $-1$；否則，觀察以 $i$ 為開頭的序列個數為

$$K_i := \frac{(n_1+\ldots+n_m-1)!}{n_1! \ldots (n_i-1)! \ldots n_m!} = K\frac{n_i}{n_1+\ldots+n_m}.$$

計算 $K_1$ 需要 $O(n)$ 時間，接著由於

$$K_i = K_1\frac{n_i}{n_1},$$

可以用 $O(n)$ 算好所有的 $K_i$。總共有 $O(n)$ 格要填，故時間複雜度為 $O(n^2)$。

### 更快的演算法

雖然不是本題的考點，由於 $k$ 最大只到 $10^9$，相較 $(n-2)!$ 不大，我們可以利用 $k$ 來得到一個接近線性的演算法。

首先，若 $m \ge 14$，則 $K_1 \ge 13! > 10^9$，故當 $m$ 夠大時可以直接填 $1$；另一方面，若 $m \le 13$，我們需要常數時間計算

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

因此字典序第 $k$ 小序列可以在 $O(\kappa n)$ 時間得到，這裡 $\kappa$ 是滿足階乘為 $10^9$ 以上的最小整數。

---

## H - ussr

---

## I - xmas
