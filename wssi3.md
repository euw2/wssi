import numpy as np
import matplotlib.pyplot as plt
import networkx as nx


def sigmoid(x):
    return 1 / (1 + np.exp(-x))


class Neuron:
    def __init__(self, n_inputs, activation_function=None):
        self.weights = np.random.randn(n_inputs) * 0.1
        self.bias = np.random.randn(1) * 0.1
        self.activation = activation_function

    def activate(self, inputs):
        z = np.dot(inputs, self.weights) + self.bias
        return self.activation(z) if self.activation else z


class Layer:
    def __init__(self, n_neurons, n_inputs_per_neuron, activation_function=None):
        self.neurons = [Neuron(n_inputs_per_neuron, activation_function) for _ in range(n_neurons)]

    def forward(self, inputs):
        return np.array([neuron.activate(inputs) for neuron in self.neurons])


class NeuralNetwork:
    def __init__(self, layer_sizes, activation_funcs=None):
        self.layers = []
        for i in range(1, len(layer_sizes)):
            activation_function = activation_funcs[i-1] if activation_funcs and i < len(layer_sizes) - 1 else None
            self.layers.append(Layer(layer_sizes[i], layer_sizes[i-1], activation_function))

    def predict(self, inputs):
        for layer in self.layers:
            inputs = layer.forward(inputs)
        return inputs


def visualize_network(network, ax, layer_sizes):
    G = nx.DiGraph()
    pos = {}
    previous_layer = None
    for i, size in enumerate(layer_sizes):
        layer_name = f"Layer {i}"
        nodes = [f"{layer_name}.{j}" for j in range(size)]
        G.add_nodes_from(nodes)
        if previous_layer:
            for prev_node in previous_layer:
                for node in nodes:
                    G.add_edge(prev_node, node)
        pos.update((node, (i, -j)) for j, node in enumerate(nodes))
        previous_layer = nodes

    nx.draw(G, pos, ax=ax, with_labels=True, node_color='red', node_size=2000, font_size=8)


layer_sizes = [3, 4, 4, 1]
activation_funcs = [None, np.tanh, sigmoid]

nn = NeuralNetwork(layer_sizes, activation_funcs)

fig, ax = plt.subplots(figsize=(14, 7))
visualize_network(nn, ax, layer_sizes)
plt.show()
