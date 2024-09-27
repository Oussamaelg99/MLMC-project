\documentclass{beamer}
\usepackage{multirow}

\begin{document}

\begin{frame}{MLMC, MLQMC and Validation Comparison}
    \begin{table}[h!]
        \centering
        \begin{tabular}{|c|c|c|c|c|c|c|}
            \hline
            \multirow{2}{*}{$\epsilon$} & \multicolumn{2}{c|}{MLMC Validation} & \multicolumn{2}{c|}{MLMC} & \multicolumn{2}{c|}{MLQMC} \\
            \cline{2-7}
             & L2 Error & Time (s) & L2 Error & Time (s) & L2 Error & Time (s) \\
            \hline
            $0.1$ & 0.005 & 10.2 & 0.004 & 12.5 & 0.003 & 9.8 \\
            \hline
            $0.01$ & 0.002 & 20.1 & 0.0015 & 25.3 & 0.0012 & 18.4 \\
            \hline
            $0.001$ & 0.0005 & 45.6 & 0.0004 & 50.7 & 0.0003 & 40.1 \\
            \hline
        \end{tabular}
        \caption{Comparison of L2 Error and Time for MLMC Validation, MLMC, and MLQMC}
    \end{table}
\end{frame}

\end{document}
