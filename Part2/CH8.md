# CH8 - 金氧半場效電晶體多級放大電路

## 差動放大器

因有兩個輸入端，且輸入訊號與二輸入端信號乘正比

* 兩個輸入都會發生雜訊，可互相抵銷
* 理想輸入無雜訊
* 其他公式請參考第10章運算放大器

\begin{circuitikz}[american]
	\node[nmos, tr circle, arrowmos] at (-1, 0) (Q1) {};
	\node[nmos, tr circle, xscale=-1, arrowmos] at (1, 0) (Q2) {};
	\draw (Q1.S) to (Q2.S);
	\draw ($0.5*(Q1.S)+0.5*(Q2.S)$) to [dcisource, l=$I_D$, fill=yellow, *-o] ++(0, -2) node[below] {$-V_{SS}$};
	\draw (Q1.G)
	  to [short] ++(-0.7, 0)
	  to [sV=$V_{i1}$, fill=yellow] ++(0, -2) node[ground] {};
	\draw (Q2.G)
		to [short] ++(0.7, 0)
		to [sV=$V_{i2}$, fill=yellow] ++(0, -2) node[ground] {};
	\draw (Q1.D)
		to [short, -*, i_<=$I_{D1}$] ++(0, 0.5) coordinate (X)
		to [C, l_=$C_1$, -o] ++(-1.5, 0) node[left] {$V_{o1}$};
	\draw (X)
	    to [R, l_=$R_{D1}$] ++(0, 1.5) coordinate (Z)
	    to [short] ++(2, 0);
	\draw (Q2.D)
		to [short, -*, i<=$I_{D2}$] ++(0, 0.5) coordinate (Y)
		to [C=$C_2$, -o] ++(1.5, 0) node[right] {$V_{o1}$};
	\draw (Y)
	    to [R=$R_{D2}$] ++(0, 1.5);
	 \draw ($(Z)+(1,0)$)
	   to [short, *-o] ++(0, 0.6) node[above] {$+V_{DD}$};
\end{circuitikz}

\begin{empheq}[box={\BgBox}]{gather}
A_{vo1} = \frac{V_{o1}}{V_{i1} - V_{i2}} = \frac{- g_m \times R_D}{2} \\
A_{vo2} = \frac{V_{o2}}{V_{i1} - V_{i2}} = \frac{  g_m \times R_D}{2} \\
A_{vT} = \frac{V_{o1} - V_{o2}}{V_{i1} - V_{i2}} = - g_m \times R_D
\end{empheq}

