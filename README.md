import pydot
from PIL import Image

# Define the UML class diagram in dot format
uml_code = """
digraph G {
    node [shape=record, fontname=Helvetica, fontsize=10];
    
    Simulator [label="{Simulator|+ price(): float\\l+ test(): void\\l+ test_convergence(): void\\l}"];
    MC [label="MC"];
    QMC [label="QMC"];
    MLMC [label="MLMC"];
    MLQMC [label="MLQMC"];
    AMLMC [label="AMLMC"];
    AMLQMC [label="AMLQMC"];
    Model [label="{Model|+ drift(): float\\l+ diffusion(): float\\l+ diffusion_d(): float\\l}"];
    GBM [label="GBM"];
    FXVolSto [label="FXVolSto"];
    Scheme [label="{Scheme|+ eval_f(): float\\l+ eval_y(): float\\l}"];
    EulerScheme [label="EulerScheme"];
    MilsteinScheme [label="MilsteinScheme"];
    FXscheme [label="FXscheme"];
    Payoff [label="{Payoff|+ eval_payoff(path): float\\l}"];
    EUCall [label="EUCall"];
    AsianCall [label="AsianCall"];
    UnOCall [label="UnOCall"];
    DigitalCall [label="DigitalCall"];
    LookbackCall [label="LookbackCall"];
    Driver [label="{Driver|+ buildSimulator(): Simulator\\l+ price(): float\\l+ compare_simulators(): void\\l}"];

    Simulator -> MC;
    Simulator -> QMC;
    Simulator -> MLMC;
    Simulator -> MLQMC;
    Simulator -> AMLMC;
    Simulator -> AMLQMC;
    Model -> GBM;
    Model -> FXVolSto;
    Scheme -> EulerScheme;
    Scheme -> MilsteinScheme;
    Scheme -> FXscheme;
    Payoff -> EUCall;
    Payoff -> AsianCall;
    Payoff -> UnOCall;
    Payoff -> DigitalCall;
    Payoff -> LookbackCall;
}
"""

# Parse the dot format
graph = pydot.graph_from_dot_data(uml_code)[0]

# Save the graph to a file
graph.write_png('uml_class_diagram.png')

# Display the image
image = Image.open('uml_class_diagram.png')
image.show()
