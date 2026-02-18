# BlameGame: Backpropagation Visualized

An interactive canvas-based visualization that lets you *feel* how backpropagation works by wiggling individual network weights and watching error attribution flow backward through a neural network.

## What is Backpropagation?

Backpropagation is basically the main trick that lets neural networks (the things powering almost all modern AI) actually learn from their mistakes — and do it efficiently.

Imagine you're teaching a very tall stack of people (layers) to guess the price of a house just by looking at a few facts like size, number of bedrooms, location, etc.

Here's how the learning usually goes wrong without backpropagation:

- The top person (the final answer layer) makes a guess → it's way off.
- They blame the person who gave them the information (the layer below).
- That person blames the one below them... and so on.
- Eventually everyone is blaming everyone else and nobody knows who should actually change their behavior.

Backpropagation fixes this blame game in a smart, organized way.

### Super simple analogy: the "wrong answer → blame backward" process

1. **Forward pass (the guess)**
   You feed the house facts into the bottom of the network.
   Every layer does some calculation → passes its answer upward.
   → Finally you get a predicted price, say $420,000.
   The true price is $500,000.
   → Error = $80,000 too low.

2. **Backward pass (the blame & learning signal)**
   Start at the top: "Hey output layer — you were $80,000 too low. That's your fault score."
   Now go backward one layer:
   - Ask: "How much did your output contribute to making the final answer too low?"
   - Using a bit of calculus (chain rule), we calculate exactly how sensitive the final error was to tiny changes in that layer's output.
   → That gives a "blame score" (technically: the **gradient**) for that layer.

   Repeat going downward through every layer:
   Each layer passes blame to the layer below it, adjusted by how much influence it had.

3. **Update step**
   Every connection (every little number/weight in the network) now gets told:
   "You contributed X amount to the error → nudge yourself a tiny bit in the opposite direction."
   (This nudge = learning rate × blame score)

Do this thousands/millions of times on lots of examples → the network gradually gets better at guessing.

### Why is this such a big deal?

Before backpropagation (1980s breakthrough), people could only really train single-layer networks.
As soon as you stacked more layers the blame became impossible to assign efficiently → deep networks were basically untrainable.

Backprop gave us an efficient way to send error signals backward through many layers at once — usually just a few lines of code mathematically, even for huge networks with billions of parameters.

### One-sentence version most people remember

> Backpropagation = take the final mistake, trace exactly how much each knob in the whole network helped cause that mistake, then gently turn every knob a tiny bit in the helpful direction — and repeat forever.

That's it. Elegant, mechanical, and ridiculously powerful when you do it at massive scale with massive data and fast computers.

## How to Use

Open `index.html` in any modern browser. Draw a digit on the 16x16 canvas, then:
- **Hover** over connections to see their weight values
- **Click and drag** (wiggle) any connection to perturb its weight
- Watch in real-time how the perturbation affects the output predictions
- Connections that cause large output changes when wiggled = high sensitivity = high "blame"
- Color coding shows gradient magnitude (red = high blame, blue = low blame)
