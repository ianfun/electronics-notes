---
CJKmainfont: "Noto Sans CJK TC"
mainfont: "Ubuntu Mono"
documentclass: report
---

# CH7 - 金氧半場效電晶體放大電路

## BJT 放大電路整理

\begin{CE}{CE(Common Emitter)}
\begin{circuitikz}
\ctikzset{resistors/scale=0.6}
\draw (0,0) node[ground] {}
  to [R=$R_E$, i<=$i_e$, -*] (0, 1)
  to [short] (-1, 1)
  to [R, l_=$r_{\pi}$, i<=$i_b$] (-1, 3)
  to [short, -o] (-3, 3) node[left] {$V_i$};
\draw (-2, 3)
  to [R, l_=$R_B$] (-2, 0) node[ground] {};
\draw (2, 3)
  to [R=$R_C$] (2, 0) node[ground] {};
\draw (0, 1)
  to [short] (1, 1);
\draw (1, 3)
  to [american controlled current source, i=$i_c$](1, 1);
\draw (1, 3)
  to [short] (1, 3)
  to [short, -o] (3, 3) node[right] {$V_o$};
\end{circuitikz}

General purpose amplifier \& Reverse phase \& High Efficiency
\end{CE}

\begin{CC}{CC(Common Collector)}
\begin{circuitikz}
\ctikzset{resistors/scale=0.6}
\draw (0, 0.8)
  to [short, *-o] ++(0.5, 0) node[right] {$V_o$};
\draw (0,0) node[ground] {}
  to [R=$R_E$, i<=$i_e$, -*] (0, 1)
  to [short] (-1, 1)
  to [R, l_=$r_{\pi}$, i<=$i_b$] (-1, 3)
  to [short, -o] (-3, 3) node[left] {$V_i$};
\draw (-2, 3)
  to [R, l_=$R_B$] (-2, 0) node[ground] {};
\draw (2, 3)
  to [R=$R_C$] (2, 0) node[ground] {};
\draw (0, 1)
  to [short] (1, 1);
\draw (1, 3)
  to [american controlled current source, i=$i_c$](1, 1);
\draw (1, 3)
  to [short] (1, 3)
  to [short] (3, 3) node[ground] {};
\end{circuitikz}

Voltage buffer(follower) \& High input resistance \& High current gain
\end{CC}

\begin{CB}{CB(Common Base)}
\begin{circuitikz}
\ctikzset{resistors/scale=0.6}
\draw (0, 0) node[left] {$V_i$}
  to [short, o-] ++(1, 0)
  to [R=$R_E$] ++(0, -2);
\draw (1, -3) node[ground] {}
  to [battery, v=$V_{EE}$] ++(0, 1);
\draw (1, 0)
  to [R=$r_e$, i<=$i_e$, *-*] ++(2, 0)
  to [R=$R_B$, i<=$i_b$] ++(0, -3) node[ground] {};
\draw (5, 0)
  to [american controlled current source, i_=$i_c$] ++(-2, 0);
\draw (5, 0)
  to [R=$R_C$] ++(0, -2)
  to [battery, v=$V_{CC}$] ++(0, -1) node[ground] {};
\draw (5, 0)
  to [short, *-o] ++(1, 0) node[right] {$V_o$};
\end{circuitikz}

Current buffer(follower) \& High output resistance \& High voltage gain \& High-frequency amplifiers
\end{CB}

常用公式整理

\begin{empheq}[box={\BgBox}]{align*}
{}& g_m = \frac{\Delta i_c}{\Delta v_{be}} = \frac{I_{CQ}}{I_{BQ}} \\
{}& r_{\pi} = \frac{V_T}{I_{BQ}} \\
{}& r_e = \frac{V_T}{I_{EQ}} \\
{}& r_o = \frac{V_A + V_{CE}}{I_C}
\end{empheq}

## MOSFET 放大電路整理

\begin{CS}{CS(Common Source)}
\begin{circuitikz}
\ctikzset{resistors/scale=0.7}
\ctikzset{batteries/scale=0.7}
\ctikzset{capacitors/scale=0.6}
\node[nmos, tr circle] at (0, 0) (A) {};
\draw (A.S) to [short] ++(0, -1.5) node[ground] {};
\draw (A.D) to [C=$C_D$, *-o] ++(1.2, 0) node[right] {$V_o$}; 
\draw (A.D) to [short, -o] ++(0, 1.5) node[above] {$+V_{DD}$};
\draw (A.G) to [short] ++(-0.7, 0) coordinate (X);
\draw (X) to [R=$R_G$] ++(0, -1.3) to [battery, v=$V_{GG}$] ++(0, -1) node[ground] {};
\draw (X) to [C=$C_G$, *-o] ++(-1.2, 0) node[left] {$V_i$};
\node[magenta, left] at (A.S) {S};
\node[magenta, left] at (A.D) {D};
\node[magenta, above] at (A.G) {G};
\end{circuitikz}
\begin{circuitikz}
\ctikzset{resistors/scale=0.7}
\ctikzset{batteries/scale=0.7}
\ctikzset{capacitors/scale=0.6}
\node[nmos, tr circle] at (0, 0) (A) {};
\draw (A.S) to [short] ++(0, -1.5) node[ground] {};
\draw (A.D) to [short, -o] ++(0, 1.5) node[above] {$+V_{DD}$};
\draw (A.G) to [short] ++(-0.7, 0) coordinate (X);
\draw (X) to [R=$R_G$] ++(0, -1.3) to [battery, v=$V_{GG}$] ++(0, -1) node[ground] {};
\node[magenta, left] at (A.S) {S};
\node[magenta, left] at (A.D) {D};
\node[magenta, above] at (A.G) {G};
\end{circuitikz}
\begin{circuitikz}
\ctikzset{resistors/scale=0.5}
\ctikzset{capacitors/scale=0.6}
\ctikzset{csources/scale=0.7}
\draw (0, -1) node[ground] {}
  to [R=$R_G$] ++(0, 2) coordinate (X)
  to [short, *-o] ++(-1, 0) node[left] {$V_i$};
\draw (X)
  to [short, -o] ++(1, 0) node[above, magenta] {G}
  to [open, v=$v_{gs}$] ++(0, -1.3)
  to [short, o-*] ++(1, 0) coordinate (Y);
\draw (Y) to [short] ++(0, -0.7) node[ground] {} node[right, magenta] {S};
\draw (Y)
  to [short, -*] ++(1, 0) coordinate(Z)
  to [short] ++(1, 0)
  to [R, l_=$\infty(r_o)$, -*] ++(0, 1.3) coordinate (F)
  to [short] ++(-1, 0) node[left, magenta] {D}
  to [american controlled current source, l_=$g_m v_{gs}$] ++(0, -1.3);
\draw (F)
  to [short, -*] ++(1.5, 0) coordinate (G)
  to [short, -o] ++(0.5, 0) node[right] {$V_o$};
\draw (G)
  to [R=$R_D$] ++(0, -2) node[ground] {};
\end{circuitikz}
\begin{gather*}
A_v = \frac{V_o}{V_i} = \frac{-i_d R_D}{v_{gs}} = \frac{-g_m \times v_{gs} \times R_D}{v_{gs}} = -g_m \times R_D (-g_m \times (R_D \parallelsum r_o))\\
R_i = R_G \\
R_o = R_o (R_o \parallelsum r_o)
\end{gather*}
\end{CS}

\begin{CD}{CD(Common Drain)}
\begin{circuitikz}
\ctikzset{resistors/scale=0.7}
\ctikzset{batteries/scale=0.7}
\ctikzset{capacitors/scale=0.6}
\node[nmos, tr circle] at (0, 0) (A) {};
\draw (A.S) to [R=$R_S$] ++(0, -1.5) node[ground] {};
\draw (A.S) to [C=$C_S$, *-o] ++(1.2, 0) node[right] {$V_o$}; 
\draw (A.D) to [short, -o] ++(0, 1.5) node[above] {$+V_{DD}$};
\draw (A.G) to [short] ++(-0.7, 0) coordinate (X);
\draw (X) to [R=$R_G$] ++(0, -1.3) to [battery, v=$V_{GG}$] ++(0, -1) node[ground] {};
\draw (X) to [C=$C_G$, *-o] ++(-1.2, 0) node[left] {$V_i$};
\node[magenta, left] at (A.S) {S};
\node[magenta, left] at (A.D) {D};
\node[magenta, above] at (A.G) {G};
\end{circuitikz}
\begin{circuitikz}
\ctikzset{resistors/scale=0.7}
\ctikzset{batteries/scale=0.7}
\ctikzset{capacitors/scale=0.6}
\node[nmos, tr circle] at (0, 0) (A) {};
\draw (A.S) to [R=$R_S$] ++(0, -1.5) node[ground] {};
\draw (A.D) to [short, -o] ++(0, 1.5) node[above] {$+V_{DD}$};
\draw (A.G) to [short] ++(-0.7, 0) coordinate (X);
\draw (X) to [R=$R_G$] ++(0, -1.3) to [battery, v=$V_{GG}$] ++(0, -1) node[ground] {};
\node[magenta, left] at (A.S) {S};
\node[magenta, left] at (A.D) {D};
\node[magenta, above] at (A.G) {G};
\end{circuitikz}
\begin{circuitikz}
\ctikzset{resistors/scale=0.5}
\ctikzset{capacitors/scale=0.6}
\ctikzset{csources/scale=0.7}
\draw (0, -1) node[ground] {}
  to [R=$R_G$] ++(0, 2) coordinate (X)
  to [short, *-o] ++(-1, 0) node[left] {$V_i$};
\draw (X)
  to [short, -o] ++(1, 0) node[above, magenta] {G}
  to [open, v=$v_{gs}$] ++(0, -1.3)
  to [short, o-*] ++(1, 0) coordinate (Y);
\draw (Y) to [R=$R_S$] ++(0, -1) node[ground] {} node[right, magenta] {S};
\draw (Y)
   to [short] ++(1, 0) 
   to [short, *-o] ++(0, -1) node[below] {$V_o$};
\draw (Y)
  to [short, -*] ++(1, 0) coordinate(Z)
  to [short] ++(1, 0)
  to [R, l_=$\infty(r_o)$, -*] ++(0, 1.3) coordinate (F)
  to [short] ++(-1, 0) node[left, magenta] {D}
  to [american controlled current source, l_=$g_m v_{gs}$] ++(0, -1.3);
\draw (F)
  to [short, -*] ++(1.5, 0) coordinate (G)
  to [short] ++(0.5, 0) node[ground] {};
\draw (G)
  to [R=$R_D$] ++(0, -2) node[ground] {};
\end{circuitikz}


\begin{gather*}
A_v = \frac{V_o}{V_i} = \frac{g_m \times v_{gs} \times R_S}{v_{gs} + g_m \times v_{gs} \times R_S} = \frac{g_m \times R_S}{1 + g_m \times R_S} (\frac{g_m \times (R_S \parallelsum r_o)}{1 + g_m \times (R_S \parallelsum r_o)}) \\
A_v \approx 1 \\
R_i = R_G \\
R_o = R_S \parallelsum \frac{1}{g_m} (R_S \parallelsum (r_o \parallelsum \frac{1}{g_m}))
\end{gather*}
\end{CD}

\begin{CG}{CG(Common Gate)}
\begin{circuitikz}
\ctikzset{resistors/scale=0.6}
\ctikzset{batteries/scale=0.7}
\ctikzset{capacitors/scale=0.6}
\node[nmos, tr circle] at (0, 0) (A) {};
\draw (A.S) to [R=$R_S$] ++(0, -1.5) node[ground] {};
\draw (A.S) to [C=$C_S$, *-o] ++(1.2, 0) node[right] {$V_o$}; 
\draw (A.D) to [R=$R_D$, -o] ++(0, 1.5) node[above] {$+V_{DD}$};
\draw (A.G) to [short] ++(-0.7, 0) coordinate (X);
\draw (X) to [R=$R_G$] ++(0, -1.3) to [battery, v=$V_{GG}$] ++(0, -1) node[ground] {};
\draw (X) to [C=$C_G$, *-] ++(-1.2, 0) 
  to[short] ++(0, -1) node[ground] {};
\draw (A.D)
  to [C=$C_D$, *-o] ++(1, 0) node[right] {$V_o$};
\node[magenta, left] at (A.S) {S};
\node[magenta, left] at (A.D) {D};
\node[magenta, above] at (A.G) {G};
\end{circuitikz}
\begin{circuitikz}
\ctikzset{resistors/scale=0.6}
\ctikzset{batteries/scale=0.7}
\ctikzset{capacitors/scale=0.6}
\node[nmos, tr circle] at (0, 0) (A) {};
\draw (A.S) to [R=$R_S$] ++(0, -1.5) node[ground] {};
\draw (A.D) to [R=$R_D$, -o] ++(0, 1.5) node[above] {$+V_{DD}$};
\draw (A.G) to [short] ++(-0.7, 0) coordinate (X);
\draw (X) to [R=$R_G$] ++(0, -1.3) to [battery, v=$V_{GG}$] ++(0, -1) node[ground] {};
\node[magenta, left] at (A.S) {S};
\node[magenta, left] at (A.D) {D};
\node[magenta, above] at (A.G) {G};
\end{circuitikz}
\begin{circuitikz}
\ctikzset{resistors/scale=0.5}
\ctikzset{capacitors/scale=0.6}
\ctikzset{csources/scale=0.7}
\draw (0, 0) node[above] {D}
  to [short, o-*] ++(0, -0.5) coordinate (X)
  to [short] ++(1.5, 0)
  to [R=$r_o$] ++(0, -3)
  to [short] ++(-1.5, 0);
\draw (X)
  to [american controlled current source, l=$g_m v_{gs}$] ++(0, -1.5) coordinate (Y)
  to [R=$\frac{1}{g_m}$] ++(0, -1.5) coordinate (Z)
  to [short, *-o] ++(0, -0.5) node[below] {S};
\draw (Z)
  to [R=$R_S$] ++(-1.5, 0) node[ground] {};
\draw (X)
  to [R=$R_D$] ++(-1.5, 0) node[ground] {};
\draw (Y)
  to [short, *-] ++(-1.5, 0) node[ground] {} node[left] {G};
\draw (Y)
  ++(-0.4, 0)
  to [open, v=$v_{gs}$] ++(0, -1.4);
\end{circuitikz}

\begin{gather*}
A_v = \frac{V_o}{V_i} = \frac{-g_m \times v_{gs} \times R_D}{-v_{gs}} = g_m \times R_D \\
R_i = R_S \parallelsum \frac{1}{g_m}\\
R_o = R_D
\end{gather*}
\end{CG}

常用公式整理

\begin{empheq}[box={\BgBox}]{gather*}
I_D = k \times (V_{GS} - V_{GSt})^2  \\
I_D = I_{DSS} \times (1 - \frac{V_{GS}}{V_{GSp}})^2 \\
g_m = 2k (V_{GSQ} - V_{GSt})  \\
g_m = \frac{2 I_{DSS}}{|V_{GSp}|} \times (1-\frac{V_{GSQ}}{V_{GSp}}) \\
g_m = 2 \sqrt{k \times I_D}  \\
\mu = g_m \times r_o  \\
r_o = \frac{V_A}{I_D}
\end{empheq}

