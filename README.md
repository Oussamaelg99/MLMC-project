
import numpy as np
from scipy.linalg import solve_banded
import matplotlib.pyplot as plt

class AsianOptionPricer:
    def __init__(self, S_max, A_max, T, K, r, sigma, N):
        self.S_max = S_max
        self.A_max = A_max
        self.T = T
        self.K = K
        self.r = r
        self.sigma = sigma
        self.N = N
        self.dt = T / N

    def price(self, S_disc, A_disc):
        M = len(S_disc) - 1
        L = len(A_disc) - 1
        dS = self.S_max / M
        dA = self.A_max / L

        # Create grid
        S = np.array(S_disc)
        A = np.array(A_disc)
        V = np.zeros((M + 1, L + 1))

        # Terminal condition
        for i in range(M + 1):
            for j in range(L + 1):
                V[i, j] = max(A[j] - self.K, 0)

        # Main loop for time stepping
        for n in range(self.N, 0, -1):
            # Coefficients for the Crank-Nicolson scheme
            alpha = 0.25 * self.dt * (self.sigma**2 * S**2 / dS**2 - self.r * S / dS)
            beta = -0.5 * self.dt * (self.sigma**2 * S**2 / dS**2 + self.r)
            gamma = 0.25 * self.dt * (self.sigma**2 * S**2 / dS**2 + self.r * S / dS)

            # Matrices for tridiagonal system
            A_mat = np.zeros((3, M + 1))
            B_mat = np.zeros((3, M + 1))

            for j in range(1, L):
                A_mat[1, :] = 1 - beta
                A_mat[0, 1:] = -alpha[:-1]
                A_mat[2, :-1] = -gamma[1:]

                B_mat[1, :] = 1 + beta
                B_mat[0, 1:] = alpha[:-1]
                B_mat[2, :-1] = gamma[1:]

                b = B_mat[1, :] * V[:, j] + B_mat[0, 1:] * V[1:, j] + B_mat[2, :-1] * V[:-1, j]

                # Apply boundary conditions
                b[0] = 0
                b[-1] = S_max - K * np.exp(-r * (N - n) * dt)

                # Solve the tridiagonal system
                V[:, j] = solve_banded((1, 1), A_mat, b)

        # Interpolate the value at the initial conditions
        S0 = S_disc[0]
        A0 = A_disc[0]

        # Find the nearest indices for interpolation
        i = np.searchsorted(S, S0) - 1
        j = np.searchsorted(A, A0) - 1

        # Bilinear interpolation
        V_0 = V[i, j] + (V[i + 1, j] - V[i, j]) * (S0 - S[i]) / dS + (V[i, j + 1] - V[i, j]) * (A0 - A[j]) / dA

        return V_0

    def plot_surface(self, V, S_disc, A_disc):
        S_grid, A_grid = np.meshgrid(S_disc, A_disc)
        fig = plt.figure()
        ax = fig.add_subplot(111, projection='3d')
        ax.plot_surface(S_grid, A_grid, V.T, cmap='viridis')
        ax.set_xlabel('Stock Price S')
        ax.set_ylabel('Average Price A')
        ax.set_zlabel('Option Value V')
        plt.show()

# Parameters
S_max = 100  # Maximum stock price
A_max = 100  # Maximum average price
T = 1.0  # Time to maturity
K = 50  # Strike price
r = 0.05  # Risk-free rate
sigma = 0.2  # Volatility
N = 100  # Number of time steps

# Discretization
S_disc = np.linspace(0, S_max, 100)  # Spatial discretization for S
A_disc = np.linspace(0, A_max, 100)  # Spatial discretization for A

# Create the pricer instance
pricer = AsianOptionPricer(S_max, A_max, T, K, r, sigma, N)

# Price the option
option_price = pricer.price(S_disc, A_disc)
print(f"The price of the Asian call option is approximately {option_price:.4f}")

# Plot the option value surface
pricer.plot_surface(option_price, S_disc, A_disc)
