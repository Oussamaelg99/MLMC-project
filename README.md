\documentclass{article}
\usepackage{graphicx}
\usepackage{subcaption}

\begin{document}

\begin{figure}[htb]
\centering
% First Plot
\begin{subfigure}[b]{0.32\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot1.pdf}
    \caption{First plot}
    \label{fig:plot1}
\end{subfigure}
\hfill % spacing between the figures
% Second Plot
\begin{subfigure}[b]{0.32\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot2.pdf}
    \caption{Second plot}
    \label{fig:plot2}
\end{subfigure}
\hfill % spacing between the figures
% Third Plot
\begin{subfigure}[b]{0.32\textwidth}
    \centering
    \includegraphics[width=\textwidth]{plot3.pdf}
    \caption{Third plot}
    \label{fig:plot3}
\end{subfigure}

\caption{Three horizontal plots}
\label{fig:three_plots}
\end{figure}

\end{document}
