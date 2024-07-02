
import subprocess

# PlantUML code
uml_code = """
@startuml
interface Simulator {
  + price(): float
  + test(): void
  + test_convergence(): void
}

class MC
class QMC
class MLMC
class MLQMC
class AMLMC
class AMLQMC

Simulator <|-- MC
Simulator <|-- QMC
Simulator <|-- MLMC
Simulator <|-- MLQMC
Simulator <|-- AMLMC
Simulator <|-- AMLQMC

interface Model {
  + drift(): float
  + diffusion(): float
  + diffusion_d(): float
}

class GBM
class FXVolSto

Model <|-- GBM
Model <|-- FXVolSto

interface Scheme {
  + eval_f(): float
  + eval_y(): float
}

class EulerScheme
class MilsteinScheme
class FXscheme

Scheme <|-- EulerScheme
Scheme <|-- MilsteinScheme
Scheme <|-- FXscheme

interface Payoff {
  + eval_payoff(path): float
}

class EUCall
class AsianCall
class Un​⬤
