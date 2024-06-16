The Euler method approximates the solution of a stochastic differential equation (SDE) like the GBM, which is defined by 
\[
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t,
\]
where \( \mu \) is the drift coefficient, \( \sigma \) is the volatility coefficient, and \( W_t \) is a standard Brownian motion. The Euler approximation for GBM over a timestep \( \Delta t \) is given by:
\[
S_{t+\Delta t} = S_t + \mu S_t \Delta t + \sigma S_t \Delta W_t,
\]
where \( \Delta W_t \sim N(0, \Delta t) \) represents the increment of the Brownian motion over the interval \( \Delta t \).

\textbf{Strong Error:} The strong convergence of the Euler method for GBM is characterized by an error bound of \( O(\sqrt{\Delta t}) \). This indicates that the average error between the numerical solution and the exact solution measured across paths scales with the square root of the timestep:
\[
\mathbb{E}\left[\sup_{0 \leq t \leq T} |S_t - \hat{S}_t|\right] = O(\sqrt{\Delta t}),
\]
where \( \hat{S}_t \) is the numerical approximation of \( S_t \) at time \( t \), and the expectation is over different realizations of the Brownian path.

\textbf{Weak Error:} The weak convergence for the Euler method is generally better, with an error rate of \( O(\Delta t) \). This error rate describes the convergence of the expected values of functionals of the solution:
\[
|\mathbb{E}[f(S_T)] - \mathbb{E}[f(\hat{S}_T)]| = O(\Delta t),
\]
for some sufficiently smooth function \( f \). This Strong error leads to an MLMC estimator that falls in the case $\beta = \gamma$ with a theoritcal complexity of $O(\epsilon^{-2.5})$

\subsection*{Milstein Scheme}
The Milstein scheme enhances the Euler method by including an additional term to account for the curvature (or the second derivative) of the diffusion term. For GBM, the Milstein update formula is:
\[
S_{t+\Delta t} = S_t + \mu S_t \Delta t + \sigma S_t \Delta W_t + \frac{1}{2} \sigma^2 S_t (\Delta W_t^2 - \Delta t),
\]
which corrects for the discretization of the stochastic integral.

\textbf{Strong Error:} The inclusion of the Itô correction term improves the strong convergence rate to \( O(\Delta t) \):
\[
\mathbb{E}\left[\sup_{0 \leq t \leq T} |S_t - \hat{S}_t|\right] = O(\Delta t),
\]
making the Milstein scheme more accurate for path-dependent scenarios.

\textbf{Weak Error:} The weak convergence rate remains \( O(\Delta t) \) as in the Euler method, but with a reduced constant factor, providing potentially more accurate estimates of expected values.

This Strong error leads to an MLMC estimator that falls in the case of $\beta > \gamma$ with a theoritical complexity of $O(\epsilon^{-2})$ which is optimal.
