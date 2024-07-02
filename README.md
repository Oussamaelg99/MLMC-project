Simulator: {
  shape: class

  +price(): float
  +test(): void
  +test_convergence(): void
}

MC: { shape: class }
QMC: { shape: class }
MLMC: { shape: class }
MLQMC: { shape: class }
AMLMC: { shape: class }
AMLQMC: { shape: class }

Simulator <|-- MC
Simulator <|-- QMC
Simulator <|-- MLMC
Simulator <|-- MLQMC
Simulator <|-- AMLMC
Simulator <|-- AMLQMC

Model: {
  shape: class

  +drift(): float
  +diffusion(): float
  +diffusion_d(): float
}

GBM: { shape: class }
FXVolSto: { shape: class }

Model <|-- GBM
Model <|-- FXVolSto

Scheme: {
  shape: class

  +eval_f(): float
  +eval_y(): float
}

EulerScheme: { shape: class }
MilsteinScheme: { shape: class }
FXscheme: { shape: class }

Scheme <|-- EulerScheme
Scheme <|-- MilsteinScheme
Scheme <|-- FXscheme

Payoff: {
  shape: class

  +eval_payoff(path): float
}

EUCall: { shape: class }
AsianCall: { shape: class }
UnOCall: { shape: class }
DigitalCall: { shape: class }
LookbackCall: { shape: class }

Payoff <|-- EUCall
Payoff <|-- AsianCall
Payoff <|-- UnOCall
Payoff <|-- DigitalCall
Payoff <|-- LookbackCall

Driver: {
  shape: class

  +buildSimulator(): Simulator
  +price(): float
  +compare_simulators(): void
}
