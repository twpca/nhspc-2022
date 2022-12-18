# 基地駐守 (base)

\textcolor{red}{\textbf{本題有 Special Judge，會依照解的好壞程度動態給分。}}

## 問題描述

烏克麗麗王國的領土以一個無向圖 (undirected graph) 來代表，圖上的每個頂點 (vertex) 代表一個基地，而圖上的邊 (edge) 為連接基地與基地之間的道路。
領土中的基地編號為 $1$ 到 $n$，每一條基地間的道路距離都恰好為 $1$ 單位，並且城巿 $i$ 一開始有駐守 $w_i$ 單位的守軍。
當一個城巿裡**恰好**有 $S$ 個單位的守軍駐守時，此基地達到完美防守的狀態，能抵擋任何外來的攻擊。
由於烏克麗麗王國即將面臨外敵的侵略，在此問題中，你的目標是協助移動烏克麗麗王國各基地的守軍，讓最多個城巿達到完美防守的狀態。

由於烏克麗麗王國在歷史上長期受到外敵的威脅，他們早已未雨綢繆地在軍隊的配置上下了不少工夫。
國防大臣告訴你，在每個城巿的距離為 $1$ 的範圍內，都至少有 $S$ 單位的守軍。
也就是說如果將基地 $u, v$ 的最短距離以 $d(u, v)$ 表示的話，對任何的城巿 $v$ 而言，以下不等式

$$\sum_{u:d(u,v) \le 1} w_u \ge S$$ 

\noindent 都成立。

為了避免守軍移動疲憊，烏克麗麗王國的國防大臣希望每單位守軍最長的移動距離不超過 $X$。
下圖為兩個 $S = 10, X = 3$ 的例子，圖上每個圓圈代表一個基地，圓圈裡的數字代表當時基地中的守軍數量。
在圖 (a) 的例子中我們可以將右下角基地的軍隊移至上方及左下方的基地，最終結果如圖 (b) 有 $2$ 個基地達到完美防守的狀態。
在圖 (c) 的例子中我們將左上、右下程式的軍隊分別移至其他兩個點，最終結果如圖 (d) 所示，一樣有 $2$ 個基地為完美防守。
上面兩例子中最長的移動距離都不超過 $3$，滿足國防大臣的希望。

\begin{minipage}{0.24\textwidth}
  \begin{figure}[H]
    \centering
    \begin{tikzpicture}[scale=0.8]
      \def \Nodes {
        1/1/2/8,
        2/0/0/7,
        3/2/0/6}
      \def \Edges {
        1/2,
        2/3,
        3/1}

      \foreach \id / \x / \y / \w in \Nodes {
        \node[draw,circle] (\id) at (\x, \y) {\w};
      }
      \foreach \x / \y / \w / \labelpos in \Edges {
        \path[draw,-,thick] (\x) -- (\y);
      }
    \end{tikzpicture}
    \caption{(a)}
  \end{figure}
\end{minipage}
\begin{minipage}{0.24\textwidth}
  \begin{figure}[H]
    \centering
    \begin{tikzpicture}[scale=0.8]
      \def \Nodes {
        1/1/2/10,
        2/0/0/10,
        3/2/0/1}
      \def \Edges {
        1/2,
        2/3,
        3/1}

      \foreach \id / \x / \y / \w in \Nodes {
        \node[draw,circle] (\id) at (\x, \y) {\w};
      }
      \foreach \x / \y / \w / \labelpos in \Edges {
        \path[draw,-,thick] (\x) -- (\y);
      }
    \end{tikzpicture}
    \caption{(b)}
  \end{figure}
\end{minipage}
\begin{minipage}{0.24\textwidth}
  \begin{figure}[H]
    \centering
    \begin{tikzpicture}[scale=0.8]
      \def \Nodes {
        1/0/0/7,
        2/2/0/6,
        3/2/2/5,
        4/0/2/2}
      \def \Edges {
        1/2,
        2/3,
        3/4,
        4/1,
        2/4}

      \foreach \id / \x / \y / \w in \Nodes {
        \node[draw,circle] (\id) at (\x, \y) {\w};
      }
      \foreach \x / \y / \w / \labelpos in \Edges {
        \path[draw,-,thick] (\x) -- (\y);
      }
    \end{tikzpicture}
    \caption{(c)}
  \end{figure}
\end{minipage}
\begin{minipage}{0.24\textwidth}
  \begin{figure}[H]
    \centering
    \begin{tikzpicture}[scale=0.8]
      \def \Nodes {
        1/0/0/10,
        2/2/0/0,
        3/2/2/10,
        4/0/2/0}
      \def \Edges {
        1/2,
        2/3,
        3/4,
        4/1,
        2/4}

      \foreach \id / \x / \y / \w in \Nodes {
        \node[draw,circle] (\id) at (\x, \y) {\w};
      }
      \foreach \x / \y / \w / \labelpos in \Edges {
        \path[draw,-,thick] (\x) -- (\y);
      }
    \end{tikzpicture}
    \caption{(d)}
  \end{figure}
\end{minipage}


現在你收到國防大臣的依賴，他們希望你給出一連串的軍隊移動計畫，目標是使最多數目的城巿達到完美防守並且盡可能讓軍隊最長的移動距離滿足大臣的要求。
在收到你的移動計畫後，他們會命令軍隊一起移動並在最短的時間內前往防守位置。
本題將依照輸出解滿足條件的程度動態給分，詳情請見「評分說明」一節敘述。

## 輸入格式

\begin{format}
\f{
$n$ $m$ $S$ $X$
$w_1$ $w_2$ $\cdots$ $w_n$
$u_1$ $v_1$
$u_2$ $v_2$
$\vdots$
$u_m$ $v_m$
}
\end{format}

* $n$, $m$ 分別代表王國中的基地以及道路數量。
* $S$ 代表一個基地達成完美防守恰好所需的守軍數量。
* $X$ 代表軍隊一天最長能移動的距離。
* $u_i$, $v_i$ 為兩基地編號，代表第 $i$ 條道路連接基地 $u_i$ 與 $v_i$。

## 輸出格式

\begin{format}
\f{
$K$ $X_a$ $O$
$a_1$ $b_1$ $c_1$
$a_2$ $b_2$ $c_2$
$\vdots$
$a_O$ $b_O$ $c_O$
}
\end{format}

* $K$, $X_a$ 為兩整數，分別代表你輸出的移動方案中滿足完美防守的基地數以及軍隊最長移動距離。若不需要做任何移動，$X_a$ 請輸出 $0$。
* $O$ 為輸出的移動操作數量。
* $a_i$, $b_i$, $c_i$ 代表第 $i$ 個操作，會將**移動前原本在** $a_i$ 基地的守軍 $c_i$ 單位移動到 $b_i$ 基地。

## 測資限制

* $1 \le n \le 500$。
* $n-1 \le m \le \frac{n(n-1)}{2}$。
* $1 \le S \le 500$。
* $0 \le w_i \le 500$。
* $5 \le X \le 500$。
* $1 \le u_i, v_i \le n$。
* 輸入的圖保證連通，沒有重邊、自環；且每個點與其相鄰點的軍隊總和 $\ge S$。
* 以上變數皆為整數。

## 評分說明

每筆輸入檔皆有一個 $K^{*}$ 代表所有操作中可能達到最多的完美防守基地數量。令

* $K_{\Delta} = K - K^{*}$，也就是輸出的解完美防守基地數比最佳解多幾個
* $X_{\Delta} = \min\{0, X_a - X\}$，也就是輸出解的最長移動距離比大臣要求多多少

則該筆測試資料的得分為：

$$\frac{1}{1.5^{K_{\Delta}} \times 3^{X_{\Delta}}}$$

乘以該筆子任務配分，並且該子任務的得分為所有輸入檔最小的得分。請注意若有以下情況則該筆輸入以 $0$ 分計：

* $O > 500n$
* 所有移動結束後，滿足完美防守的基地數量或移動最長距離不滿足輸出宣稱的 $K$, $X_a$
* 輸出的基地編號不存在
* 移動不合法，例如 $a_i = b_i$，或者 $c_i \le 0$。
* 從基地 $x$ 出發的的守軍單位總和比 $w_x$ 多（沒軍隊可以移動）
* 不滿足輸出格式，例如有多餘非空白字元的輸出

## 範例測試

## 評分說明

本題共有五組子任務，條件限制如下所示。
每一組可有一或多筆測試資料，取該組所有測試資料中最低分為該子任務得分。

|  子任務  |  分數  | 額外輸入限制 |
| :------: | :----: | ------------ |
| 1 | {{score.subtask1}} | 給定的圖為一條直線 |
| 2 | {{score.subtask2}} | 給定的圖為一棵樹 |
| 3 | {{score.subtask3}} | $X \ge 20$ |
| 4 | {{score.subtask4}} | $X \ge 7$ |
| 5 | {{score.subtask5}} | 無額外限制 |

備註：

* 直線: 一個連通無向圖，除兩點度數為 $1$ 以外其餘點的度數皆為 $2$
* 樹：一個連通且無環的無向圖
