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
class UnOCall
class DigitalCall
class LookbackCall

Payoff <|-- EUCall
Payoff <|-- AsianCall
Payoff <|-- UnOCall
Payoff <|-- DigitalCall
Payoff <|-- LookbackCall

class Driver {
  + buildSimulator(): Simulator
  + price(): float
  + compare_simulators(): void
}

@enduml
"""

# Write PlantUML code to a file
with open('diagram.uml', 'w') as f:
    f.write(uml_code)

# Generate the diagram
subprocess.run(["plantuml", "diagram.uml"])
