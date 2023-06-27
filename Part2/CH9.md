# CH9 - 金氧半場效電晶體數位電路

## 數位積體電路的比較

* BJT

  操作速度快、高驅動電流
  1. RTL(Resustor Transistor Logic): 最早發展，已淘汰
  2. DTL(Diode-Transistor Logic): 用二極體取代電阻，已淘汰
  3. TTL(Transistor-Transistor Logic): 用BJT取代二極體，發展最多樣與完整，主要應於SSI、MSI
  4. ECL(Emitter Coupled Logic): 又稱 CML(Current-Mode Logic)，**電流驅動力和超作速度最高**。
* MOSFET

  低功耗、製成簡單、高製造密度
  1. PMOS: 因傳送的是電洞，速度慢於NMOS
  2. NMOS: 傳送電子，**製造密度是MOSSFET最高**、**製成最簡單**
  3. CMOS(Complementary MOS)/互補式MOSSFET: 結合PMOS與NMOS的數位電路，是所有數位電路中**最低功耗、最省電、最高抗雜訊**
* BiCMOS

  結合BJT和CMOS的數位電路，BiCMOS(BJT+CMOS)為BiMOS(BJT+MOSFET)的一種

## MOSFET 數位電路

### CMOS 反項器

\begin{circuitikz}[american]
\node[ieeestd not port] at (0, -1) (N) {};
\node[draw] at (0, 1) {IEEE standard not port};
\node[left] at (N.in) {A};
\node[right] at (N.out) {$Y$};

\node[draw] at (8, 1) {CMOS 簡化版};
\node[pmos] at (8, -0.7) (C){};
\node[nmos] at (8, -2) (D){};
\node[ground] at (D.S) {};
\draw [short] (C.G) -- (D.G);
\draw (8, -1.4)
  to[short, *-o] ++(1, 0);
\draw[short, -o] (C.S) -- ++(0, 0.1) node[above] {$V_{DD}$};
\node[] at (9.7, -1.4) {$Y$};
\draw [short, -](C.G) -- ++(0, -0.7)
  to[short, *-o] ++(-0.5, 0) node[left]{A};  

\ctikzset{tripoles/mos style/arrows}
\node[draw] at (4, 1) {CMOS 反相器};
\node[pmos,nocircle,arrowmos] at (4, -0.7) (A){};
\node[nmos,nocircle,arrowmos] at (4, -2) (B){};
\node[ground] at (B.S) {};
\draw [short] (A.G) -- (B.G);
\draw [short, -] (A.G) -- ++(0, -0.7)
  to[short, *-o] ++(-1, 0) node[left]{A};  
\draw[short, -o] (A.S) -- ++(0, 0.1) node[above] {$V_{DD}$};
\draw (4, -1.4)
  to[short, *-o] ++(1, 0);
\node[] at (5.7, -1.4) {$Y$};
\end{circuitikz}
\ctikzset{tr circle=true}

CMOS反相器工作區

\begin{tikzpicture}[scale=0.8]
\draw[->] (0, 0) -- (0, 7.5) node[above] {A};
\draw[->] (0, 0) -- (7.5, 0) node[right] {Y};
\node[] at (0, -0.4) {$0$};
\draw[dashed, red] (1.5, 0) -- ++(0, 7);
\node[] at (1.5, -0.4) {$V_1$};
\draw[dashed, red] (3.2, 0) -- ++(0, 7);
\node[] at (3.2, -0.4) {$V_2$};
\draw[dashed, red] (3.8, 0) -- ++(0, 7);
\node[] at (3.8, -0.4) {$V_3$};
\draw[dashed, red] (5.5, 0) -- ++(0, 7);
\node[] at (5.5, -0.4) {$V_4$};
\draw [blue, thick] plot [smooth] coordinates {(0, 7)(3, 6)(3.5, 3.5)(4, 1)(7, 0)};
\end{tikzpicture}

輸入    | PMOS      | NMOS
------- | -------   | -------
$A < V_1$ | 線性電阻區 | 截止區
$V_1 < A < V_2$ | 線性電阻區 | 夾止飽和區
$V_2 < A < V_3$ | 夾止飽和區 | 夾止飽和區
$V_3 < A < V_4$ | 夾止飽和區 | 線性電阻區
$V4 < A$ | 截止區 | 線性電阻區

## 三態反閘

* 邏輯符號

\begin{circuitikz}
\node[ieeestd not port] at (0, 0)(A) {};
\draw (A.up) -- ++(0, 0.8) node[above] {$EN$};
\draw (A.in) -- ++(-0.5, 0) node[left] { $A$ };
\draw (A.out) -- ++(0.5, 0) node[right] { $Y$ };
\end{circuitikz}

* 真值表

控制 EN | 輸入A   | 輸出Y
------- | ------- | -------
0       | x       | Z
1       | 0       | 1
1       | 1       | 0 

* CMOS電路圖

\begin{circuitikz}
\node[pmos, tr circle] at (2, 1.5) (A) {};
\node[nmos, tr circle] at (2, -1.5) (B) {};
\node[pmos, tr circle] at (4.2, 1.5) (C) {};
\node[nmos, tr circle] at (4.2, -1.5) (D) {};
\draw (0, 0)
  coordinate (X)
  to [short] ++(0, 1.5)
  to [short] (A.G);
\draw (X)
  to [short] ++(0, -1.5)
  to [short] (B.G);
\draw (C.G) node[left] {$\overline{EN}$};
\draw (D.G) node[left] {$EN$};
\draw (A.S) to [short, -o] ++(0, 0.2) node[above] {$V_{DD}$};
\node [ground] at (B.S) {};
\draw (A.D) to[short] (C.D);
\draw (B.D) to[short] (D.D);
\draw (C.S) to [short] ++(1, 0) to [short] ++(0, -2.5);
\draw (D.S) to [short] ++(1, 0) to [short] ++(0, 2.5);
\draw ($0.5*(C.S)+0.5*(D.S)+(1, 0)$) to [short, *-o] ++(1, 0) node[right] {$Y$};
\end{circuitikz}

## 緩衝閘

* 輸出=輸入，$Y=A$
* 功能：信號還原放大和時間延遲
* 邏輯符號 $Y=A$

\begin{circuitikz}
\node[ieeestd buffer port] at (0, 0) (A) {};
\draw (A.in) -- ++(-0.5, 0) node[left] { $A$ };
\draw (A.out) -- ++(0.5, 0) node[right] { $Y = A$ };
\end{circuitikz}

* $Y=\overline{\overline{A}}=A$

\begin{circuitikz}
\node[ieeestd not port] at (0, 0) (A) {};
\draw (A.in) -- ++(-0.5, 0) node[left] { $A$ };
\node[ieeestd not port] at (2, 0) (B) {};
\draw (A.out) -- (B.in);
\draw (A.out) ++(0.2, 0) to [short, *-] ++(0, -0.8) to [short] ++(1, 0) node[right] {$\overline{A}$};
\draw (B.out) -- ++(0.5, 0) node[right] { $Y = A$ };
\end{circuitikz}

* CMOS電路圖

\begin{circuitikz}
\node [pmos, tr circle] at (2, 1) (A) {};
\node [nmos, tr circle] at (2, -1) (B) {};
\node [pmos, tr circle] at (4.3, 1) (C) {};
\node [nmos, tr circle] at (4.3, -1) (D) {};
\draw (0, 0)
  to [short, o-*] ++(0.7, 0) coordinate (X)
  to [short] ++(0, 1)
  to [short] ++(0.5, 0)
  to (A.G);
\draw (X)
  to [short] ++(0, -1)
  to [short] ++(0.5, 0)
  to (B.G);
\draw (A.D) -- (B.D);
\draw ($0.5*(A.D)+0.5*(B.D)$) to [short, *-o] ++(0.5, 0) node[above] {$\overline{A}$}
to [short, -*] ++(0.5, 0) coordinate (Y)
to[short] ++(0, 1)
  to [short] ++(0.5, 0)
  to (C.G);
  \draw (Y)
  to [short] ++(0, -1)
  to [short] ++(0.5, 0)
  to (D.G);
\draw (C.D) -- (D.D);
\draw ($0.5*(D.D)+0.5*(C.D)$) to [short, *-o] ++(0.5, 0) node[above] {$Y$};
\node[ground] at (B.S) {};
\node[ground] at (D.S) {};
\draw(A.S)[short, -o] to ++(0, 0.2) coordinate(CV);
\draw(C.S)[short, -o] to ++(0, 0.2) coordinate(VC);
\node[above] at (CV) {$V_{DD}$};
\node[above] at (VC) {$V_{DD}$};
\end{circuitikz}

## 第摩根定理

\begin{empheq}[box={\BgBox}]{align}
\overline{A + B} = \overline{A} \cdot \overline{B} \\
\overline{A \cdot B} = \overline{A} + \overline{B}
\end{empheq}

## 虛擬NMOS數位電路

虛擬NMOS(Psedo NMOS)用PMOS主動式負載取代電阻

\begin{circuitikz}[american]
\node[pmos] at (0, 0) (A) {}; 
\draw[short] (A.G) to [short] ++(-0.3, 0) node[ground] {};
\draw[short, -o] (A.S) to [short] ++(0, 0.1) node[above] {$V_{DD}$};
\draw[short, -*] (A.D) to [short] ++(0, -0.5) to [short] ++(1, 0) node[right]{Y};
\draw[short] (A.D) to [short] ++(0, -1);
\node[ground] at (0, -2.28) {};
\node[draw] at (0, -2) { NMOS 數位電路 };
\end{circuitikz}

\begin{circuitikz}[american]
\node[pmos, tr circle] at (1, -1) (A) {}; 
\draw[short] (A.G) to [short] ++(-0.3, 0) node[ground] {};
\draw[short] (A.D)
  to [short] ++(0, -0.5)
  to [short, *-o] ++(0.5, 0) node [right]{$Y=A + \overline{B}C$};
\draw[short] (A.S)
  to[short, -o] ++(0, 0.5) node[above] {$V_{DD}$};

\node[nmos, tr circle] at (1, -3) (X) {};
\node[nmos, tr circle] at (1, -5) (Y) {};
\node[nmos, tr circle] at (-0.7, -5) (Z) {};
\draw[short] (X.S)
  to [short] (Y.D);
\draw[short] (Z.D)
  to [short, -*] ++(1.7, 0);
\node[left] at (X.G) {$\overline{A}$};
\node[left] at (Y.G) {$\overline{C}$};
\node[left] at (Z.G) {$B$};
\draw[short] (Z.S)
  to[short, -*] (Y.S)
  to[short] ++(0, -0.2) node[ground] {};
\end{circuitikz}

## CMOS數位電路

CMOS數位電路上面用PMOS，下面用NMOS

\begin{empheq}[box={\BgBox}]{align}
\text{PMOS布林代數式} = Y(\overline{A}, \overline{B}, \overline{C}, \cdots)\\
\text{NMOS布林代數式} = \overline{Y({A}, {B}, {C}, \cdots)}
\end{empheq}

\begin{circuitikz}[american]
\node[pmos, tr circle] at (-0.5, 0) (A) {};
\node[pmos, tr circle] at (1, 1) (B) {};
\node[pmos, tr circle] at (1, -1) (C) {};
\node[left] at (A.G) {$\overline{A}$};
\node[left] at (B.G) {${B}$};
\node[left] at (C.G) {$\overline{C}$};
\draw[short] (B.D) -- (C.S);
\draw[short] (A.D)
  to [short] ++(0, -1)
  to [short, -*] ++(1.5, 0)
  to [short] ++(0, -0.5)
  to [short, *-o] ++(0.5, 0) node [right]{$Y=A + \overline{B}C$};
\draw[short] (A.S)
  to[short] ++(0, 1)
  to[short, -*] ++(1.5, 0)
  to[short, -o] ++(0, 0.5) node[above] {$V_{DD}$};

\node[nmos, tr circle] at (1, -3) (X) {};
\node[nmos, tr circle] at (1, -5) (Y) {};
\node[nmos, tr circle] at (-0.7, -5) (Z) {};
\draw[short] (X.S)
  to [short] (Y.D);
\draw[short] (Z.D)
  to [short, -*] ++(1.7, 0);
\node[left] at (X.G) {$\overline{A}$};
\node[left] at (Y.G) {$\overline{C}$};
\node[left] at (Z.G) {$B$};
\draw[short] (Z.S)
  to[short, -*] (Y.S)
  to[short] ++(0, -0.2) node[ground] {};
\end{circuitikz}
