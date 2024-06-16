\section{Extensions}
\subsection{Randomized MLMC}
The RMLMC method enhances the traditional MLMC by using a probability distribution \( p_\ell \) to randomly select the simulation levels for each sample, aiming to maintain an unbiased estimator while potentially reducing computational costs:

\[
Y = \frac{1}{N} \sum_{n=1}^N \frac{1}{p_{\ell(n)}} (P_{\ell(n)} - P_{\ell(n)-1})
\]

In this setup, \( P_{\ell} \) represents the approximation at level \( \ell \), with \( \ell(n) \) indicating the randomly chosen level for the nth sample, based on the distribution \( p_\ell \). This formula ensures that the expectation of \( Y \) is aligned with the true expectation \( E[P] \), upholding an unbiased estimator across all levels.

The selection of \( p_\ell \) is critical and typically crafted to be inversely proportional to the variance of the level differences, adjusted by their computational costs:

\[
p_\ell = \frac{\sqrt{V_\ell / C_\ell}}{\sum_{k=0}^\infty \sqrt{V_k / C_k}}
\]

This strategic configuration draws more samples from levels where the variance, when adjusted for cost, is comparatively high, thereby aiming to improve the efficiency of variance reduction. However, the success of this strategy hinges critically on the accurate estimation of both variance \( V_\ell \) and cost \( C_\ell \), which can be challenging in complex simulations. Misestimations here can lead to inefficient computational resource allocation, detracting from the theoretical benefits of RMLMC.

Furthermore, the method's reliance on these accurate estimates introduces significant practical challenges, particularly in fields like financial modeling or advanced engineering simulations, where inputs can be highly variable and the systems themselves complex. The complexity in calculating and dynamically adjusting \( p_\ell \) may hinder the method's applicability in fast-paced or resource-limited settings, where simpler, more predictable approaches might be preferred.

In essence, while RMLMC offers a mathematically elegant solution to improving Monte Carlo simulations by integrating a probabilistic method of level selection, its practical effectiveness is bounded by the accuracy of variance and cost estimations and the complexity of its implementation. This method may find its best use in controlled environments where these parameters can be carefully managed, such as in detailed academic studies or specialized industrial research projects where computational overhead is less of a concern.

\subsection{Multilevel Richardson Romberg}
The Multilevel Richardson-Romberg (ML2R) method, detailed by Lemaire and Pagès in 2016, integrates Richardson extrapolation into MLMC to further reduce biases in estimates. This approach significantly improves the precision of calculations, crucial in areas such as financial derivatives pricing:
\[ E[P] \approx \sum_{\ell=0}^{L} w_\ell E[P_\ell], \]
where \( w_\ell \) are specifically designed weights to minimize higher-order biases, providing highly accurate and reliable estimates (\textit{Lemaire \& Pagès, 2016}).
\textbf{Theoretical Explanation and Complexity of the Multilevel Richardson-Romberg (ML2R) Method}

\textbf{Core Concepts:}
\begin{itemize}
    \item \textit{Bias Decomposition}: Assumes a systematic expansion of the bias \( E[Y_h] - E[Y_0] \) in powers of the discretization step \( h \), enabling higher-order bias corrections.
    \item \textit{Variance Reduction}: Utilizes differences between consecutive discretization levels to reduce variance, lessening the number of high-resolution simulations needed.
    \item \textit{Richardson-Romberg Extrapolation}: Uses the expansion of the bias error to eliminate higher-order terms systematically, increasing accuracy.
    \item \textit{MLMC Methodology}: Averages differences across multiple discretization levels to significantly reduce variance with fewer fine-resolution simulations.
\end{itemize}

\textbf{Mathematical Formulation:}
The ML2R estimator is given by:
\[
I_{h,R,q}^N = \frac{1}{N_1} \sum_{k=1}^{N_1} Y_{h_1}^{(k)} + \sum_{j=2}^R W_j \left( \frac{1}{N_j} \sum_{k=1}^{N_j} (Y_{h_j}^{(k)} - Y_{h_{j-1}}^{(k)}) \right)
\]
where \( R \) is the number of levels, \( N_j \) is the number of simulations at level \( j \), \( W_j \) are weights designed to optimize bias reduction, and \( h_j \) are the discretization steps, typically \( h_j = M^{-j+1}h \) for some \( M > 1 \).

\textbf{Complexity Analysis:}
The complexity of the ML2R estimator improves as the target error \( \epsilon \) decreases. The complexity for achieving a root mean squared error (RMSE) \( \epsilon \) is:
\[
\text{Cost(ML2R)} = 
\begin{cases} 
\epsilon^{-2} & \text{if } \beta > 1, \\
\epsilon^{-2} \log(1/\epsilon) & \text{if } \beta = 1, \\
\epsilon^{-2} e^{-\frac{1-\beta}{\sqrt{\alpha}}\sqrt{2 log(1/\epsilon)log(M)}}  & \text{if } \beta < 1.
\end{cases}
\]
where \( \beta \) is the strong convergence rate parameter \( \| Y_h - Y_0 \|_2 = O(h^\beta) \).

\textbf{Comparative efficiency}
\textbf{For \(\beta < 1\)}: ML2R exhibits significant superiority over MLMC, with efficiency increasing exponentially as \(\epsilon \to 0\). The improvement factor is given by:
\[
\epsilon^{-\frac{1}{\sqrt{-\beta \alpha}}} \exp\left(-\frac{1-\beta}{\alpha \sqrt{2 \log(M)}} \log(1/\epsilon)\right),
\]
which becomes greater than 1 when \(\epsilon \leq 2^{-\frac{2}{\alpha}}\).

\textbf{For \(\beta = 1\)}: ML2R outperforms MLMC by a factor of \(\log(1/\epsilon)\), showcasing its enhanced performance as precision requirements increase.

\textbf{For \(\beta > 1\)}: Both estimators achieve a convergence rate of \(\epsilon^{-2}\), akin to an unbiased Monte Carlo simulation. However, numerical experiments suggest that ML2R has a lower constant in its error term compared to MLMC, indicating better computational efficiency even when theoretical convergence rates are identical.
\subsection{Multilevel Quasi Monte Carlo}
The \textit{Multilevel Quasi-Monte Carlo} (MLQMC) method significantly enhances the efficiency of financial simulations by integrating the strengths of \textit{Quasi-Monte Carlo} (QMC) techniques with the \textit{Multilevel Monte Carlo} (MLMC) approach. This method efficiently computes various types of options, including European, Asian, lookback, barrier, and digital options, leveraging a sophisticated randomized rank-1 lattice rule within the QMC framework to drastically cut computational costs compared to using MLMC or QMC methods independently.

Central to MLQMC is the Milstein discretization of stochastic differential equations (SDEs), which is crucial for option pricing. This discretization maintains the necessary weak convergence for MLMC and enhances the first-order strong convergence, critical for the method's efficiency. The computational complexities are as follows:
\begin{itemize}
    \item Traditional Monte Carlo methods typically exhibit \(O(\epsilon^{-3})\),
    \item MLMC using Euler-Maruyama discretization reduces this to \(O(\epsilon^{-2} (\log \epsilon)^2)\),
    \item MLMC with Milstein discretization optimizes further to \(O(\epsilon^{-2})\).
\end{itemize}

The randomized rank-1 lattice rule in the QMC part of MLQMC distributes quasi-random points across the unit hypercube to minimize integration errors over the complex, high-dimensional spaces typical in financial applications. This even distribution is crucial for reducing the numerical integration discrepancy, represented mathematically as:
\[
x_i = \left\{ \frac{i \cdot z}{N} \right\}
\]
where \(i\) ranges from 0 to \(N-1\), \(z\) is a generating vector in a d-dimensional space, and \(N\) is the number of points, with the curly braces indicating the fractional part to ensure all points remain within the unit hypercube.

A notable aspect of the MLQMC method is its ability to handle path-dependent financial derivatives more effectively. The computational savings are quantified as approximately five to ten times lower than standard QMC, primarily because the variance of the highest discretization levels (where QMC is most effective) is significantly reduced. This variance reduction is crucial in financial applications where the payoff depends sensitively on the path properties of the underlying assets.

Research shows that MLQMC can reduce computational costs by approximately 5 to 10 times compared to traditional QMC methods when applied to European options. However, the studies also highlight limitations, including the challenge of effective randomization in QMC, which is crucial for maintaining unbiased estimates. Moreover, the effectiveness of the MLQMC may vary depending on the smoothness and dimensionality of the underlying problem. The method's performance heavily depends on the correct configuration of the lattice rule and the nature of the payoff functions involved.

Despite its advantages, the MLQMC method faces challenges such as the need for effective randomization in QMC to ensure robust error estimates and the lack of comprehensive theoretical support for its efficacy across all types of payoffs and conditions. The current research indicates promising directions for extending MLQMC to more complex payoffs and enhancing its theoretical framework to fully exploit its potential in reducing computational demands in financial engineering .


\subsection{MLMC With antithetic treatement}





\section{Applications}
\subsection{digital payoff}
The key mathematical formulations for the estimations of payoffs on coarse (\(P_c\)) and fine (\(P_f\)) paths are outlined as follows:

1. \textbf{Fine Path Payoff (\(P_f\))}: The payoff for the fine path, \(P_f\), incorporates adjustments using conditional expectation techniques, which are essential for managing the final timestep more accurately. For a digital option, this is typically handled using the Euler-Maruyama discretization. Given \( Ŝ_{N-1} \), the numerical approximation just before the final timestep, the payoff is defined using:
   \[
   P_f = 25 \exp(-rT) \Phi\left(\frac{Ŝ_{N-1} + a(Ŝ_{N-1}) h - K}{b(Ŝ_{N-1}) \sqrt{h}}\right),
   \]
   where \( \Phi \) denotes the cumulative distribution function of the standard normal distribution, \( r \) is the risk-free rate, \( T \) is the maturity, \( K \) is the strike price, and \( h \) represents the timestep size.

2. \textbf{Coarse Path Payoff (\(P_c\))}: The coarse path payoff, \(P_c\), utilizes a similar approach but integrates a Brownian increment from the second-last fine timestep. This increment corresponds to the first half of the final coarse timestep, aligning the coarse path's conditional distribution with the fine path to improve accuracy and reduce estimation variance:
   \[
   P_c = 25 \exp(-rT) \Phi\left(\frac{Ŝ_{N-2} + a(Ŝ_{N-2}) h_{l-1} + b(Ŝ_{N-2}) \sqrt{h} - K}{b(Ŝ_{N-2}) \sqrt{h}}\right),
   \]
   where \( h_{l-1} \) denotes the timestep size at the previous level.

These mathematical expressions effectively smooth the payoff function, mitigating the variance spikes typically caused by the discontinuity at the threshold. This advanced handling enables MLMC to maintain a complexity of \(O(\epsilon^{-2})\), rendering it a superior computational strategy compared to traditional Monte Carlo simulations under equivalent accuracy conditions. This innovative approach exemplifies the flexibility and efficiency of MLMC in managing complex, high-dimensional problems in financial engineering and beyond.


\section{Implementation}
\subsection{Crude Monte Carlo}
In the implementation of the Crude Monte Carlo estimator, the process begins by setting up simulations with varying numbers of paths and timestep sizes to examine their impact on the outcomes. A grid of errors for each parameter combination is generated, providing a comprehensive overview of performance across different settings. Following this, a straightforward optimization procedure is employed to identify the optimal pair of timestep size and number of paths that achieves the desired accuracy, denoted by epsilon. Finally, tests are conducted using these optimized parameters to validate the effectiveness of the chosen settings, ensuring efficient use of computational resources while maintaining the accuracy of the simulations. This approach underscores a methodical strategy to balance computational efficiency with precision in stochastic evaluations.
\subsection{Quasi Monte Carlo}
In the implementation of the Quasi Monte Carlo method, the approach mirrors that of the Crude Monte Carlo by generating an error grid to evaluate the effectiveness of different numbers of paths (N) and timestep sizes (h), and then using optimization techniques to pinpoint the optimal parameters for achieving the desired accuracy, epsilon. A significant enhancement in this methodology involves the simulation of Brownian paths, where two distinct methods are employed: the forward method and the Brownian bridge method. The Brownian bridge method, in particular, is recognized for its superior ability to reduce variance in the simulation outcomes, leading to better root mean square error (RMSE) performance. This is primarily due to its more effective handling of the path-dependent properties of the financial instruments being modeled.

Additionally, the Quasi Monte Carlo method utilizes the Sobol sequence generator from the Scipy QMC module, which is especially notable for its use of scrambled Sobol points. Scrambling is a crucial technique that improves the uniformity and distribution properties of the Sobol sequences, thereby enhancing their effectiveness in reducing the discrepancy of the sequence. This is vital for achieving more reliable and accurate simulation results. The combination of the Brownian bridge method for path generation and the scrambled Sobol sequences substantially enhances the robustness and accuracy of the Quasi Monte Carlo simulations
\subsection{Multilevel Monte Carlo}
In the implementation of the Multilevel Monte Carlo (MLMC) estimator as detailed in \cite{Pages2016}, the estimator is strategically designed to optimize the allocation of computational resources across various levels of accuracy, achieving a prescribed error $\epsilon$ efficiently. This optimization process is fundamentally grounded in solving an optimal allocation problem that dictates essential parameters: the number of levels $R$, the starting timestep size $h$, and the distribution of computational efforts in terms of the number of paths $N$ simulated at each level. The generic problem is the following with $\pi representing the set of parameters$

(\pi(\varepsilon), N(\varepsilon))=\underset{\left\|I_\pi^N-I_0\right\|_2 \leqslant \varepsilon}{\operatorname{argmin}} \operatorname{Cost}\left(I_\pi^N\right) .

This problems is not attainable directly and is solved in two steps

Step 1: Minimization of the effort $\phi$ over all allocation policies $q=\left(q_j\right)_{1 \leqslant j \leqslant R}$ (as a function of a fixed bias parameter $h$ ). In practice, They minimize an upper-bound $\bar{\phi}$ of the effort $\phi$
$$
q^*=\underset{q \in \mathcal{S}_{+}(R)}{\operatorname{argmin}} \bar{\phi}\left(\pi_0, q\right), \quad \text { where } \quad \phi(\pi) \leqslant \bar{\phi}(\pi), \quad \text { and } \quad \phi^*\left(\pi_0\right)=\phi\left(\pi_0, q^*\right) .
$$

This phase is solved in Theorem 3.6 In the paper ( and an explicit expression for $\bar{\phi}$ is provided). The quantity $\phi^*\left(\pi_0\right)$ is called the optimally allocated effort (with a slight abuse of terminology since $\bar{\phi}$ is only an upper bound of $\phi)$.

Step 2: Minimization of the resulting cost as a function of the remaining parameters $\pi_0$ for a prescribed $\mathbf{L}^2$-error $\varepsilon>0$ (and specification of the resulting size of the simulation and its cost):
$$
\pi_0(\varepsilon)=\underset{\substack{\pi_0 \in \Pi_0 \\\left|\mu\left(\pi_0, q^*\right)\right|<\varepsilon}}{\operatorname{argmin}}\left(\frac{\phi^*\left(\pi_0\right)}{\varepsilon^2-\mu^2\left(\pi_0, q^*\right)}\right), \quad N\left(\pi_0(\varepsilon)\right)=\frac{\phi^*\left(\pi_0(\varepsilon)\right)}{\kappa\left(\pi_0(\varepsilon), q^*\right)\left(\varepsilon^2-\mu^2\left(\pi_0, q^*\right)\right)} .
$$

They solve asymptotically when $\varepsilon$ goes to 0 in two sub-steps. First they consider a fixed depth $R$ (with general refiners) which provides a closed form for $h^*(\varepsilon)$. Secondly, They let $R$ vary as a function of $\varepsilon$ (only for geometric refiners $n_i=M^{i-1}$ ). This leads to the main result of the paper Theorem 3.12 which yields a closed form for $R^*(\varepsilon)$ (and $\left.N^*(\varepsilon)\right)$ and the various asymptotics for the cost, depending on $\beta$ and other structural parameters.

The resulting parameters are summerized in the following table :



% table herer **********************



Note that these parameters rely only on the structural parameters of the problem that need to be either determined theoritically which could be the case for $\alpha$ and $\beta$ estimated as for the case of $V1$ the constant of weak error decay and $\theta = \frac{V1}{var(Y_0)}$. V1 is estimated using :
$$\widehat{V}_1(h)=\left(1+M_{\max }^{-\frac{\beta}{2}}\right)^{-2} h^{-\beta}\left\|Y_h-Y_{\frac{h}{M_{\max }}}\right\|_2^2$$
The only problem is the constant $c_1$ which according to the authors it is challenging to estimate and thus it is usually
implemented in a blind way by considering the coefficient equal to 1. 
Although the estimator is robust in theory, its practical implementation acknowledges certain limitations, such as the difficulty in precisely estimating the constant $c_1$
c, which is set to 1 for simplification. This simplification assumes that the problems are properly non-dimensionalized, which may limit the estimator's applicability to a specific class of problems that fit this assumption.

A final detail is that the origin implementation in the paper relies on optimizing the geometric refiners base M ($h_l = M^{-l}$) however as this very costly we fixed $M = 2$.

All this in mind the implementation is very straightforward. We start by an intial run to with $N0$ samples and $L$ levels in order to estimate the different structural parameters and then we calculate the parameters fo the estimator and finish by simulating the necessary number of samples at each level of refinement and aggregating them to get the final price.
\begin{algorithm}[H]
\caption{MLMC Estimator Implementation}

\DontPrintSemicolon
\KwInput{Desired error $\epsilon$, initial number of samples $N_0$, levels $L$}
\KwOutput{Estimated final price}

\BlankLine
\tcp{Initialization}
Perform initial run with $N_0$ samples and $L$ levels\;
Estimate structural parameters (e.g., variances, computational costs)\;

\BlankLine
\tcp{Parameter Calculation}
Calculate parameters for the estimator (optimal $N_l$ for each level $l$)\;

\BlankLine
\tcp{Simulation}
\For{$l = 1$ to $L$}{
    Simulate $N_l$ samples at level $l$\;
}

\BlankLine
\tcp{Aggregation}
Aggregate results from all levels to compute the final price\;

\end{algorithm}



\subsection{Multilevel Quasi Monte Carlo}
In the Multilevel Quasi Monte Carlo approach, Sobol sequence generators with varying dimensions are used at different simulation levels to ensure that sampling while mainting low discrepency properties of the sequences. This method is coupled with the Brownian bridge technique for path generation, which helps in effectively reducing the variance associated with the payoff calculations. Together, these techniques enable precise and efficient simulations, with results from each level combined to derive the final estimation. This setup ensures that the MLQMC method achieves accurate results while maintaining computational efficiency.
\subsection{Adapted Multilevel Monte Carlo}
The adaptive version of the Multilevel Monte Carlo (MLMC) algorithm, as detailed in the Acta Numerica paper, offers a dynamic and efficient way to achieve desired accuracy levels in numerical simulations. Starting with three levels (L=2) and an initial target of \(N_0\) samples at levels \( \ell = 0, 1, 2 \), this algorithm reflects its adaptiveness by iteratively adjusting the number of levels and the number of samples per level based on ongoing assessments of variance and convergence.

The algorithm continuously evaluates the need for additional samples at each level during the simulation process. This determination is based on the variance estimates and the computational costs associated with each level, ensuring that resources are allocated efficiently. Throughout the simulation, it updates estimates for the variance \(V_\ell\) at each level \(\ell\). These variances are then used to compute the optimal number of samples \(N_\ell\) for each level according to the formula:
$$
N_{\ell}=\left\lceil 2 \varepsilon^{-2} \sqrt{V_{\ell} / C_{\ell}}\left(\sum_{\ell=0}^L \sqrt{V_{\ell} C_{\ell}}\right)\right\rceil,
$$
where \(\epsilon\) is the target root mean square error, \(V_\ell\) is the variance at level \(\ell\), and \(C_\ell\) is the cost of sampling at level \(\ell\).

The algorithm includes a robust test for weak convergence to ensure that the error between the estimated solution and the true solution is within the desired bounds. It tests if the absolute difference between the expected values of consecutive levels is less than \(\epsilon/\sqrt{2}\), adjusting the total number of levels if necessary:
\[
\left|\frac{E[P_L - P_{L-1}]}{2^\alpha - 1}\right| < \frac{\epsilon}{\sqrt{2}},
\]
where \(\alpha\) is a convergence parameter. This test may use extrapolation from the differences at previous levels to provide a more stable estimate of the remaining error.

If convergence criteria are not met, the algorithm increases the number of levels \(L\) and initializes the target for the new highest level, ensuring that the precision improves with each iteration until the desired MSE of less than \(\epsilon^2\) is achieved.

This adaptive MLMC algorithm is designed to optimize computational efforts by intelligently allocating resources where they are most needed, based on real-time assessments of variance and error convergence. This approach not only enhances the efficiency of simulations but also adapts to the complexity of the underlying models, crucial for achieving high accuracy in stochastic simulations. However, a notable limitation of this adaptive approach is its reduced potential for parallelization. Unlike more straightforward MLMC methods that allow for extensive parallel execution, the sequential dependency on variance updates and convergence tests in the adaptive approach restricts its ability to fully exploit parallel computing architectures. This limitation can impact the overall efficiency, especially in environments where parallel processing could significantly speed up computations.

\begin{algorithm}[H]
\caption{Adaptive MLMC Algorithm}
\SetAlgoLined
\KwResult{Achieve MSE less than \(\epsilon^2\)}
 Initialize \(L=2\) and set \(N_0\) samples at levels \( \ell = 0, 1, 2\)\;
 \While{extra samples needed}{
  Evaluate extra samples at each level\;
  Compute/update estimates for \(V_\ell\), \( \ell = 0, \ldots, L\)\;
  Define optimal \(N_\ell\), \( \ell = 0, \ldots, L\) using $N_{\ell}=\left\lceil 2 \varepsilon^{-2} \sqrt{V_{\ell} / C_{\ell}}\left(\sum_{\ell=0}^L \sqrt{V_{\ell} C_{\ell}}\right)\right\rceil,$
  Test for weak convergence: \( \left|\frac{E[P_L - P_{L-1}]}{2^\alpha - 1}\right| < \frac{\epsilon}{\sqrt{2}} \)\;
  \If{not converged}{
   Set \(L := L+1\)\;
   Initialize target \(N_L\)\;
  }
 }
\end{algorithm}

\subsection{Adapted Multilevel Quasi Monte Carlo}
%\usepackage{algorithm2e}
Building on the previously described QMC sequence generation methods, the Adaptive Multilevel Quasi-Monte Carlo (AMLQMC) algorithm incorporates these techniques within a dynamic multilevel framework to significantly enhance computational efficiency and accuracy in stochastic simulations. The Important point to mention here is that each level is initialized with it's own Sobol sequence sampler that implements scarmbeling to ensuring the robustness of the randomization. Every time a step needs to be simulated the appropriate sampler is called and continues generating from the last point in order to ensure the low discrepancy properties of all the sequences. 

The AMLQMC algorithm begins with initializing a few levels, typically \(L=2\), and setting a very small initial set size, \(N_{\ell}=1\), for the first few levels (\(\ell = 0, 1, 2\)). This setup allows the algorithm to start its iterative process of adjusting and refining the number of points and levels based on the variance and error convergence assessments conducted throughout the simulation process.

The algorithm dynamically adjusts the number of QMC points at each level, \(N_{\ell}\), to optimize integration accuracy and computational cost. This optimization is guided by an evaluation of 32 randomized set averages, which helps in accurately estimating the variance, \(V_{\ell}\), at each level. The algorithm continually checks if the total variance across all levels meets a specific threshold, \(\frac{1}{2} \epsilon^2\), to ensure the desired accuracy.

When additional precision is needed, or if the total variance threshold is not met, the algorithm identifies the level \(\ell^*\) where increasing \(N_{\ell}\) would yield the most significant reduction in variance relative to computational cost. This is determined by:
\[
\ell^* = \arg\max_{\ell} \frac{V_{\ell}}{N_{\ell}C_{\ell}},
\]
where \(C_{\ell}\) represents the computational cost at level \(\ell\). The number of points at this level is then doubled to rapidly decrease the variance.

The process iterates—evaluating set averages, recalculating variances, and adjusting point numbers—until the weak convergence test confirms that the integration error is within the specified limits, or until the total variance condition is satisfied. If necessary, the algorithm increases the number of levels and initializes the new level with a minimal set size to continue refining the results.

\begin{algorithm}[H]
\caption{Adaptive MLQMC Algorithm}
\SetAlgoLined
\KwResult{Achieve MSE less than \(\epsilon^2\)}
 Initialize \(L=2\) and set \(N_{\ell}=1\) for \(\ell = 0, 1, 2\)\;
 \While{not converged}{
  Evaluate 32 set averages for each level with new or changed \(N_{\ell}\)\;
  Compute new or updated estimates for \(V_{\ell}\)\;
  Test whether total variance condition \(\sum_{\ell=0}^L V_{\ell} \leq \frac{1}{2} \epsilon^2\) is satisfied\;
  \eIf{total variance too large}{
   Determine \(\ell^*\) defined by \(\ell^* = \arg\max_{\ell} \frac{V_{\ell}}{N_{\ell}C_{\ell}}\)\;
   Double \(N_{\ell^*}\)\;
   }{
   Test for weak convergence\;
   \If{not converged}{
    Set \(L := L+1\), initialize \(N_{L}=1\)\;
    }
   }
 }
\end{algorithm}

The main Shortfcomings of this algorith is unlike it's non adaptive counter part it doesn't allow for exploitating the full capabalities of parallelization as each iteration depends on the variance estimates from the one before it. Besides, The randomization make the simulation less efficient as each time instead of simulating one run of the algorithm we need to simulate 32 which leads to diminishing performance especially for larger prescribed errors.

\end{document}
