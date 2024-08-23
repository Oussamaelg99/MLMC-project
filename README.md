\documentclass{article}
\usepackage{graphicx}
\usepackage{subcaption}

\begin{document}

\begin{figure}[ht]
\centering
% First Row
\begin{subfigure}[b]{0.45\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot1.pdf}
    \caption{First plot}
    \label{fig:plot1}
\end{subfigure}
\hfill % spacing
\begin{subfigure}[b]{0.45\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot2.pdf}
    \caption{Second plot}
    \label{fig:plot2}
\end{subfigure}

% Second Row
\begin{subfigure}[b]{0.45\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot3.pdf}
    \caption{Third plot}
    \label{fig:plot3}
\end{subfigure}
\hfill % spacing
\begin{subfigure}[b]{0.45\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot4.pdf}
    \caption{Fourth plot}
    \label{fig:plot4}
\end{subfigure}

\caption{Four plots arranged in a 2x2 grid}
\label{fig:four_plots}
\end{figure}

\end{document}
