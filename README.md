
\documentclass{beamer}

\begin{document}

\begin{frame}{Context for Pricing Method}
    \textbf{Underlying Asset:}
    \[
    dX_t = \mu(X_t, t) dt + \sigma(X_t, t) dW_t
    \]

    \textbf{Discretization Scheme:}
    \[
    X_{t_{k+1}} = f(X_{t_k}, \Delta t, \Delta W_k, \dots)
    \]
    where \( \Delta t = M^{-\ell} \), and the path is denoted by \( S^{(\ell)} = \{ X_{t_0}, X_{t_1}, \dots, X_{t_{M^\ell}} \} \).

    \textbf{Payoff Function:}
    \[
    \text{Payoff} = f(S^{(\ell)}) = P^{(\ell)}
    \]

    \textbf{Pricing Objective:}
    \[
    \text{Price} = \mathbb{E}[P^{(\ell)}]
    \]
\end{frame}

\end{document}
