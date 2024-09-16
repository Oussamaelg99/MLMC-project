
\documentclass{beamer}

\begin{document}

\begin{frame}{Parallelization in MLMC}

\textbf{Three-Layer Parallelization}:
\begin{itemize}
    \item \textbf{Parallelism within samples:} 
    - Each sample is split across multiple processors for efficient local computation.
    
    \item \textbf{Parallelism across samples:} 
    - Independent samples are distributed across processors for concurrent execution.
    
    \item \textbf{Parallelism across levels:} 
    - Different levels (coarse to fine) are processed simultaneously to maximize computational resource use.
\end{itemize}

\textbf{Dynamic Scheduling Algorithm}:
\begin{itemize}
    \item Distributes tasks dynamically to balance workload in real-time.
    \item Prioritizes finer levels while allowing concurrent execution across levels.
    \item Reduces idle time and improves scalability, even with short execution times per sample.
\end{itemize}

\textbf{Master-Slave Architecture}:
\begin{itemize}
    \item \textbf{Master:} Assigns tasks and manages communication.
    \item \textbf{Slaves:} Perform computations in dynamic batches, minimizing communication overhead.
\end{itemize}

\end{frame}

\end{document}
