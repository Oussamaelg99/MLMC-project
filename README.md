import numpy as np
from scipy.fft import fft
from scipy.integrate import quad

class HestonModel:
    def __init__(self, S0, K, T, r, kappa, theta, sigma, rho, v0):
        self.S0 = S0
        self.K = K
        self.T = T
        self.r = r
        self.kappa = kappa
        self.theta = theta
        self.sigma = sigma
        self.rho = rho
        self.v0 = v0

    def characteristic_function(self, phi):
        xi = self.kappa - self.rho * self.sigma * 1j * phi
        d = np.sqrt(xi**2 + self.sigma**2 * (1j * phi + phi**2))
        g = (xi - d) / (xi + d)
        
        C = (self.r * 1j * phi * self.T +
             (self.kappa * self.theta) / (self.sigma**2) *
             ((xi - d) * self.T - 2 * np.log((1 - g * np.exp(-d * self.T)) / (1 - g))))
        
        D = ((xi - d) / (self.sigma**2) * 
             ((1 - np.exp(-d * self.T)) / (1 - g * np.exp(-d * self.T))))
        
        cf = np.exp(C + D * self.v0 + 1j * phi * np.log(self.S0 / self.K))
        return cf

    def price_european_call_fft(self, alpha=1.5, N=4096, B=1000):
        eta = B / N
        lambd = 2 * np.pi / B
        b = np.log(self.S0 / self.K) - N / 2 * lambd
        u = np.arange(1, N + 1, dtype=np.float64)
        vj = eta * (u - 1)

        # Calculate the characteristic function values
        phi_vj = self.characteristic_function(vj - (alpha + 1) * 1j)
        phi_vj *= np.exp(-self.r * self.T) / (alpha**2 + alpha - vj**2 + 1j * (2 * alpha + 1) * vj)
        
        w = eta * np.exp(-1j * b * vj) * phi_vj
        w[0] *= 0.5  # Adjust the first term

        # Perform FFT
        fft_w = fft(w).real

        # Calculate option price
        strikes = np.exp(b + lambd * (np.arange(N) - 1))
        call_prices = np.exp(-alpha * strikes) / np.pi * fft_w

        # Interpolate to find the price for the desired strike K
        call_price = np.interp(self.S0 / self.K, strikes, call_prices)
        return call_price

    def price_up_and_out_call_fdm(self, H, M=100, N=100):
        S_max = 2 * self.S0
        v_max = 2 * self.v0
        dS = S_max / M
        dv = v_max / N
        dt = self.T / N

        S_grid = np.linspace(0, S_max, M + 1)
        v_grid = np.linspace(0, v_max, N + 1)
        V = np.zeros((M + 1, N + 1))

        # Boundary conditions
        for i in range(M + 1):
            V[i, -1] = max(S_grid[i] - self.K, 0) if S_grid[i] < H else 0
        for j in range(N + 1):
            V[0, j] = 0
            V[-1, j] = 2 * S_max - self.K * np.exp(-self.r * (self.T - j * dt))

        # Iterate backward in time
        for j in range(N - 1, -1, -1):
            for i in range(1, M):
                delta_S = (V[i + 1, j + 1] - V[i - 1, j + 1]) / (2 * dS)
                gamma_S = (V[i + 1, j + 1] - 2 * V[i, j + 1] + V[i - 1, j + 1]) / (dS**2)
                delta_v = (V[i, j + 2] - V[i, j]) / (2 * dv)
                gamma_v = (V[i, j + 2] - 2 * V[i, j + 1] + V[i, j]) / (dv**2)
                V[i, j] = V[i, j + 1] + dt * (
                    -0.5 * v_grid[j] * S_grid[i]**2 * gamma_S
                    - self.rho * self.sigma * v_grid[j] * S_grid[i] * delta_S * delta_v
                    - 0.5 * self.sigma**2 * v_grid[j] * gamma_v
                    + self.r * S_grid[i] * delta_S
                    - self.r * V[i, j + 1]
                )
        return np.interp(self.S0, S_grid, V[:, 0])

# Example usage
S0 = 100
K = 100
T = 1
r = 0.05
kappa = 2.0
theta = 0.02
sigma = 0.2
rho = -0.7
v0 = 0.04
H = 120

heston = HestonModel(S0, K, T, r, kappa, theta, sigma, rho, v0)
european_call_price = heston.price_european_call_fft()
up_and_out_call_price = heston.price_up_and_out_call_fdm(H)

print("European Call Option Price under Heston Model using FFT: ", european_call_price)
print("Up-and-Out Call Option Price under Heston Model using FDM: ", up_and_out_call_price)
