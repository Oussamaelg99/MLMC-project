from graphviz import Digraph

dot = Digraph(comment='Class Diagram')

# Simulator Interface
dot.node('Simulator', '<<Interface>>\nSimulator\n- NumberGenerator\n- scheme\n- payoff\n+ price(): float\n+ test(): void\n+ test_convergence(): void')

# Simulator Implementations
simulators = ['MC', 'QMC', 'MLMC', 'MLQMC', 'AMLMC', 'AMLQMC']
for sim in simulators:
    dot.node(sim, sim)
    dot.edge(sim, 'Simulator')

# Model Interface
dot.node('Model', '<<Interface>>\nModel\n+ drift(): float\n+ diffusion(): float\n+ diffusion_d(): float')

# Model Implementations
models = ['GBM', 'FXVolSto']
for model in models:
    dot.node(model, model)
    dot.edge(model, 'Model')

# Scheme Interface
dot.node('Scheme', '<<Interface>>\nScheme\n- model\n- samplers\n+ eval_f(): float\n+ eval_y(): float')

# Scheme Implementations
schemes = ['EulerScheme', 'MilsteinScheme', 'FXscheme']
for scheme in schemes:
    dot.node(scheme, scheme)
    dot.edge(scheme, 'Scheme')

# Payoff Interface
dot.node('Payoff', '<<Interface>>\nPayoff\n+ eval_payoff(path): float')

# Payoff Implementations
payoffs = ['EUCall', 'AsianCall', 'UnOCall', 'DigitalCall', 'LookbackCall']
for payoff in payoffs:
    dot.node(payoff, payoff)
    dot.edge(payoff, 'Payoff')

# Driver Class
dot.node('Driver', 'Driver\n+ buildSimulator(): Simulator\n+ price(): float\n+ compare_simulators(): void')

# Render the diagram
dot.render('class_diagram', format='png', view=True)
