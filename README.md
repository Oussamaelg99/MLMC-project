
\documentclass{article}
\usepackage{tikz}
\usepackage{tikz-uml}

\begin{document}

\begin{tikzpicture}

\umlclass[type=interface]{Simulator}{
    - NumberGenerator \\
    - scheme \\
    - payoff
}{
    + price(): float \\
    + test(): void \\
    + test_convergence(): void
}

\umlclass[type=interface]{Model}{
    + drift(): float \\
    + diffusion(): float \\
    + diffusion_d(): float
}

\umlclass[type=interface]{Scheme}{
    - model \\
    - samplers
}{
    + eval_f(): float \\
    + eval_y(): float
}

\umlclass[type=interface]{Payoff}{
    + eval_payoff(path): float
}

\umlclass{MC}{}{}
\umlclass{QMC}{}{}
\umlclass{MLMC}{}{}
\umlclass{MLQMC}{}{}
\umlclass{AMLMC}{}{}
\umlclass{AMLQMC}{}{}

\umlclass{GBM}{}{}
\umlclass{FXVolSto}{}{}

\umlclass{EulerScheme}{}{}
\umlclass{MilsteinScheme}{}{}
\umlclass{FXscheme}{}{}

\umlclass{EUCall}{}{}
\umlclass{AsianCall}{}{}
\umlclass{UnOCall}{}{}
\umlclass{DigitalCall}{}{}
\umlclass{LookbackCall}{}{}

\umlclass{Driver}{
}{
    + buildSimulator(): Simulator \\
    + price(): float \\
    + compare_simulators(): void
}

% Relationships
\umlinherit[geometry=|-|]{MC}{Simulator}
\umlinherit[geometry=|-|]{QMC}{Simulator}
\umlinherit[geometry=|-|]{MLMC}{Simulator}
\umlinherit[geometry=|-|]{MLQMC}{Simulator}
\umlinherit[geometry=|-|]{AMLMC}{Simulator}
\umlinherit[geometry=|-|]{AMLQMC}{Simulator}

\umlinherit[geometry=|-|]{GBM}{Model}
\umlinherit[geometry=|-|]{FXVolSto}{Model}

\umlinherit[geometry=|-|]{EulerScheme}{Scheme}
\umlinherit[geometry=|-|]{MilsteinScheme}{Scheme}
\umlinherit[geometry=|-|]{FXscheme}{Scheme}

\umlinherit[geometry=|-|]{EUCall}{Payoff}
\umlinherit[geometry=|-|]{AsianCall}{Payoff}
\umlinherit[geometry=|-|]{UnOCall}{Payoff}
\umlinherit[geometry=|-|]{DigitalCall}{Payoff}
\umlinherit[geometry=|-|]{LookbackCall}{Payoff}

\end{tikzpicture}

\end{document}
