# 算樹 (tree)

## 問題描述

設 $T$ 為一棵有 $n$ 個節點的樹，節點編號 $1, 2, \ldots, n$，其中 $n \ge 3$。
$T$ 的 Prüfer 序列可由執行以下步驟 $n-2$ 次得到：

1. 找出編號最小的葉節點（即相鄰的邊數為 $1$ 的點）$u$，並設 $u$ 與 $v$ 相鄰。
1. 記下 $v$ 並從 $T$ 裡去掉 $u$ 及邊 $uv$。

\noindent 過程中依序記下的 $n-2$ 個節點就是 $T$ 的 Prüfer 序列。

考慮以下的樹：

\begin{figure}[h]
   \centering
   \begin{tikzpicture}
      \def \Vertices {
         1/0/0,
         2/-2/2,
         3/4/0,
         4/2/0,
         5/2/2,
         6/-4/0,
         7/-2/0,
         8/0/2}
      \def \Edges {
         1/4,
         1/7,
         1/8,
         2/7,
         3/4,
         4/5,
         6/7}
      \foreach \u / \x / \y in \Vertices {
         \node[draw, circle] (\u) at (\x, \y) {\u};
      }
      \foreach \u / \v in \Edges {
         \path[draw, thick] (\u) -- (\v);
      }
   \end{tikzpicture}
\end{figure}

\noindent 根據生成 Prüfer 序列的步驟，先將編號最小的葉節點 $2$ 及邊 $27$ 去掉，並記下與之相鄰的點 $7$ 得到

\begin{figure}[h]
   \centering
   \begin{tikzpicture}
      \def \Vertices {
         1/0/0,
         3/4/0,
         4/2/0,
         5/2/2,
         6/-4/0,
         7/-2/0,
         8/0/2}
      \def \Edges {
         1/4,
         1/7,
         1/8,
         3/4,
         4/5,
         6/7}
      \foreach \u / \x / \y in \Vertices {
         \node[draw, circle] (\u) at (\x, \y) {\u};
      }
      \foreach \u / \v in \Edges {
         \path[draw, thick] (\u) -- (\v);
      }
   \end{tikzpicture}
\end{figure}

\noindent 接著再將上圖編號最小的葉節點 $3$ 及邊 $34$ 去掉，並記下與之相鄰的點 $4$。
如此重複直到最後剩下兩個點 $1$ 與 $8$，過程中依序記下的 $7, 4, 4, 1, 7, 1$，即為這棵樹的 Prüfer 序列。

\begin{figure}[h]
   \centering
   \begin{tikzpicture}
      \def \Vertices {
         1/0/0,
         8/0/2}
      \def \Edges {
         1/8}
      \foreach \u / \x / \y in \Vertices {
         \node[draw, circle] (\u) at (\x, \y) {\u};
      }
      \foreach \u / \v in \Edges {
         \path[draw, thick] (\u) -- (\v);
      }
   \end{tikzpicture}
\end{figure}

已知 $T$ 每個節點的度數 (degree) 為 $d_1, d_2, \ldots, d_n$，其中 $d_i$ 為點 $i$ 的度數，請求出 $T$ 所有可能的 Prüfer 序列中，字典序第 $k$ 小的。
如果 $k$ 大於 $T$ 可能的 Prüfer 序列數，請輸出 $-1$。

## 輸入格式

\begin{format}
\f{
$n$ $k$
$d_1$ $d_2$ $\ldots$ $d_n$
}
\end{format}

* $n$ 代表 $T$ 的節點數。
* $k$ 代表找出的 Prüfer 序列，字典序由小到大的排行。
* $d_i$ 代表節點 $i$ 的度數。

## 輸出格式

如果符合條件的 Prüfer 序列有 $k$ 個以上，請輸出

\begin{format}
\f{
$p_1$
$p_2$
$\vdots$
$p_{n-2}$
}
\end{format}

其中 $p_1, p_2, \ldots, p_{n-2}$ 皆為整數，代表字典序第 $k$ 小的 Prüfer 序列。否則，請輸出

\begin{format}
\f{
$-1$
}
\end{format}

## 測資限制

* $3 \le n \le 10^3$。
* $1 \le k \le 10^9$。
* $1 \le d_i \le n-1$。
* $d_1 + d_2 + \ldots + d_n = 2n-2$。
* 以上變數皆為整數。

## 範例測試

## 評分說明

本題共有三組子任務，條件限制如下所示。
每一組可有一或多筆測試資料，該組所有測試資料皆需答對才會獲得該組分數。

|  子任務  |  分數  | 額外輸入限制 |
| :------: | :----: | ------------ |
| 1 | {{score.subtask1}} | $n \le 8$ |
| 2 | {{score.subtask2}} | $d_1 = d_2 = 1, d_3 = d_4 = \ldots = d_n = 2$ |
| 3 | {{score.subtask3}} | 無額外限制 |
