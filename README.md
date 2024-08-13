MLMC project for Monte Carlo Projuct course validation


\documentclass{article}
\usepackage{amsmath}
\usepackage{array}
\usepackage{booktabs}

\begin{document}

\begin{table}[h!]
\centering
\caption{Mathematical Expressions for Various Financial Instruments}
\begin{tabular}{>{\raggedright\arraybackslash}m{3cm} >{\raggedright\arraybackslash}m{10cm}}
\toprule
\textbf{Instrument/Payoff} & \textbf{Mathematical Expression} \\
\midrule
\textbf{European Call Option} & 
\[
\max(S_T - K, 0)
\]
where \( S_T \) is the underlying asset price at maturity, and \( K \) is the strike price. \\
\addlinespace

\textbf{Discrete Asian Option} & 
\[
\max\left(\frac{1}{n} \sum_{i=1}^{n} S_{t_i} - K, 0\right)
\]
where \( S_{t_i} \) are the asset prices at discrete observation times \( t_1, t_2, \dots, t_n \), and \( K \) is the strike price. \\
\addlinespace

\textbf{Barrier Option (Up-and-Out)} & 
\[
\begin{cases} 
\max(S_T - K, 0) & \text{if } S_t < B \text{ for all } t \leq T \\
0 & \text{if } S_t \geq B \text{ at any time } t \leq T 
\end{cases}
\]
where \( S_T \) is the asset price at maturity, \( K \) is the strike price, and \( B \) is the barrier level. \\
\addlinespace

\textbf{Forward Accumulator} & 
\[
\sum_{i=1}^{n} \max(S_{t_i} - K, 0)
\]
where \( S_{t_i} \) are the underlying asset prices at each accumulation date \( t_i \), and \( K \) is the strike price. The payoff accumulates based on the prices at discrete times. \\
\addlinespace

\textbf{Target Accumulator} & 
\[
\min\left(\sum_{i=1}^{n} \max(S_{t_i} - K, 0), T_A\right)
\]
where \( T_A \) is the target accumulation level, \( S_{t_i} \) are the asset prices at each accumulation date \( t_i \), and \( K \) is the strike price. The payoff accumulates up to a specified target level. \\
\bottomrule
\end{tabular}
\end{table}

\end{document}
