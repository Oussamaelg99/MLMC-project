
import matplotlib.pyplot as plt
import networkx as nx

# Define the UML class diagram
uml_classes = {
    "Simulator": ["MC", "QMC", "MLMC", "MLQMC", "AMLMC", "AMLQMC"],
    "Model": ["GBM", "FXVolSto"],
    "Scheme": ["EulerScheme", "MilsteinScheme", "FXscheme"],
    "Payoff": ["EUCall", "AsianCall", "UnOCall", "DigitalCall", "LookbackCall"],
    "Driver": []
}

# Create a directed graph
G = nx.DiGraph()

# Add nodes and edges
for parent, children in uml_classes.items():
    G.add_node(parent, shape='rect')
    for child in children:
        G.add_node(child, shape='rect')
        G.add_edge(parent, child)

# Define node positions
pos = nx.spring_layout(G)

# Draw nodes and edges
nx.draw(G, pos, with_labels=True, arrows=True, node_size=3000, node_color='lightblue', font_size=10, font_weight='bold')

# Draw the labels for the nodes
nx.draw_networkx_labels(G, pos, labels={node: node for node in G.nodes()}, font_size=10, font_weight='bold')

# Display the graph
plt.title("UML Class Diagram")
plt.show()
