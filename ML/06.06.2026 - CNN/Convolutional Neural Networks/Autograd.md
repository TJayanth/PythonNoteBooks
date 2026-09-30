# Autograd.ipynb — Notebook Summary

## Cell 1 (markdown)
**Framing question: how does a DL framework auto-compute gradients?**
- Poses the guiding question the notebook will answer: how frameworks compute gradients for arbitrary computation graphs.
- Sets the stage for exploring PyTorch's autograd engine.
- No code — pure orientation/motivation text.

**Key Concepts**
- Automatic differentiation
- Computation graphs

**Q&A**
- Q: What question does this notebook set out to answer? A: How a deep learning framework automatically computes gradients for any arbitrary computation graph.
- Q: Why is this question hard in general? A: Because real models have millions of parameters and complex, dynamically built graphs, so gradients can't be derived by hand.

## Cell 2 (markdown)
**Defines autograd and lists its three core ideas.**
- States autograd is an automatic differentiation engine that applies the chain rule over a dynamically built computation graph.
- Key idea 1: the graph is built at runtime (as operations execute), not pre-compiled.
- Key idea 2: gradients flow backward from the output to the inputs.
- Key idea 3: this backward flow is implemented via reverse-mode differentiation.

**Key Concepts**
- Reverse-mode automatic differentiation
- Dynamic computation graph
- Chain rule

**Q&A**
- Q: What is autograd, in one sentence? A: An engine that computes gradients automatically by applying the chain rule over a graph built at runtime.
- Q: What does "graph built at runtime" mean? A: The computation graph is constructed as operations are executed (dynamic), rather than defined ahead of time.

## Cell 3 (code — actually explanatory text, not valid Python)
**Manual derivative example for y = x² + 3x, motivating why autograd is needed.**
- Walks through manually differentiating y = x² + 3x to get dy/dx = 2x + 3.
- Notes manual differentiation is feasible for one variable but "impossible to scale to millions of parameters."
- This is prose written in a `python` cell — it is **not valid, runnable Python** (it would raise a `SyntaxError` if executed).
- Serves as the motivating problem that autograd solves.

**Key Concepts**
- Manual differentiation
- Scalability limits of manual calculus

**Q&A**
- Q: What is the manual derivative of y = x² + 3x? A: dy/dx = 2x + 3.
- Q: Why can't this manual approach scale to real neural networks? A: Real models have millions of parameters, making hand-derived gradients impractical.
- Q: Would this cell run without error if executed as Python? A: No — it's descriptive text, not valid syntax, so it would raise a SyntaxError.

## Cell 4 (code — explanatory text/table, not valid Python)
**Table describing the three things autograd tracks: Tensor, Operation, Graph.**
- Tensor holds data; Operation produces an output tensor; Graph connects operations together.
- States the important distinction: "Autograd does NOT differentiate code. It differentiates operations."
- Again written as prose/markdown table inside a `python`-tagged cell — not executable as-is.

**Key Concepts**
- Tensor
- Operation graph
- Differentiating operations vs. differentiating source code

**Q&A**
- Q: What three components does autograd track? A: Tensors (data), Operations (produce outputs), and the Graph (connects operations).
- Q: Does autograd differentiate the Python code itself? A: No — it differentiates the sequence of tensor operations recorded on the graph.

## Cell 5 (code)
**First working autograd example: y = x² + 3x, then `y.backward()`.**
- Creates a scalar leaf tensor `x = torch.tensor(2.0, requires_grad=True)`.
- Computes `y = x**2 + 3*x`, then calls `y.backward()` to trigger backpropagation.
- Prints `y` and `x.grad`, confirming the gradient equals `2(2)+3 = 7`.
- Library/API used: `torch`, `requires_grad`, `.backward()`, `.grad`.

**Key Concepts**
- Leaf tensor
- `requires_grad`
- `.backward()`
- Gradient accumulation in `.grad`

**Q&A**
- Q: What value does `x.grad` hold after running this cell? A: 7.0 (since dy/dx = 2x+3 = 2·2+3 = 7).
- Q: What triggers gradient computation here? A: Calling `y.backward()` on the scalar output `y`.

## Cell 6 (code — explanatory text, not valid Python)
**Explains what happens internally when `backward()` is called.**
- `x` is a leaf tensor (no parents in the graph).
- Operations dynamically build the graph as they execute.
- `y` stores a reference to its creator operation via `grad_fn`.
- `backward()` starts from the scalar output, applies the chain rule, and accumulates gradients into `.grad` — this is reverse-mode autodiff.

**Key Concepts**
- Leaf tensor vs. non-leaf tensor
- `grad_fn`
- Reverse-mode autodiff

**Q&A**
- Q: What is a "leaf tensor"? A: A tensor created directly by the user (not the output of an operation), such as `x` here.
- Q: What does `grad_fn` represent? A: A reference to the operation that produced the tensor, used to walk the graph backward.

## Cell 7 (code)
**Inspects `y.grad_fn` and its `next_functions`, with an ASCII diagram of the graph.**
- Prints `y.grad_fn` (e.g., `AddBackward0`) and `y.grad_fn.next_functions`.
- Includes an ASCII diagram showing `y` → `AddBackward0` → `PowBackward0` (x²) and `MulBackward0` (3x).
- Illustrates that PyTorch's graph nodes are backward-op objects, not the forward ops themselves.
- The diagram portion is illustrative text, not executable code.

**Key Concepts**
- `grad_fn` graph traversal
- Backward-op nodes (`AddBackward0`, `PowBackward0`, `MulBackward0`)

**Q&A**
- Q: What does `y.grad_fn.next_functions` show? A: The immediate predecessor backward functions that produced the inputs to `y`'s operation (here, `PowBackward0` and `MulBackward0`).
- Q: Why is the graph shaped like an upside-down tree from `y`? A: Because reverse-mode autodiff walks backward from the output through each operation that contributed to it.

## Cell 8 (code — explanatory text, not valid Python)
**Introduces the chain-rule example: z = (x² + 3x)².**
- Sets up a nested function to demonstrate multi-step backpropagation through composed operations.
- Bridges from the single-operation example (Cell 5) to a two-level composition.

**Key Concepts**
- Function composition
- Chain rule

**Q&A**
- Q: What new function is introduced here? A: z = (x² + 3x)², i.e., z = y² where y = x² + 3x.
- Q: Why use a composed function instead of a single operation? A: To show how the chain rule propagates gradients through multiple stacked operations.

## Cell 9 (code)
**Runs the composed example: computes z = y², calls `z.backward()`, prints `x.grad`.**
- Builds `y = x**2 + 3*x` then `z = y**2`, and calls `z.backward()`.
- Prints the resulting `x.grad`, demonstrating gradients flow through both operations automatically.
- Ends with the takeaway comment that "Autograd is just a very fast, very disciplined chain rule engine."

**Key Concepts**
- Multi-step reverse-mode differentiation
- Chain rule automation

**Q&A**
- Q: What does `z.backward()` compute here? A: dz/dx, propagated through both z=y² and y=x²+3x via the chain rule.
- Q: What is the notebook's summary characterization of autograd? A: A fast, disciplined chain-rule engine.

## Cell 10 (code — explanatory text, not valid Python)
**States the chain-rule identity used: dz/dx = dz/dy · dy/dx.**
- Names explicitly the mathematical rule autograd applied in the previous cell.
- Reinforces the connection between the code output and the underlying calculus.

**Key Concepts**
- Chain rule formula: dz/dx = (dz/dy)(dy/dx)

**Q&A**
- Q: What identity explains the gradient computed in Cell 9? A: dz/dx = dz/dy · dy/dx.
- Q: Which part of this identity does `PowBackward0` correspond to? A: dz/dy — the local derivative of z = y² with respect to y.

## Cell 11 (code — explanatory text/table, not valid Python)
**Table comparing forward-mode vs. reverse-mode differentiation and why DL uses reverse-mode.**
- Forward-mode is better for few inputs/many outputs; reverse-mode is better for many parameters/one output.
- States deep learning = millions of parameters + a single scalar loss, so reverse-mode is optimal.

**Key Concepts**
- Forward-mode differentiation
- Reverse-mode differentiation
- Why reverse-mode suits deep learning

**Q&A**
- Q: Why does deep learning prefer reverse-mode autodiff? A: Because models have many parameters but a single scalar loss, and reverse-mode is efficient for that many-inputs/one-output shape.
- Q: When would forward-mode be preferable instead? A: When there are few inputs but many outputs.

## Cell 12 (code)
**Demonstrates gradient accumulation: calling `.backward()` twice adds to `.grad`.**
- Computes `y1 = x**2`, calls `.backward()` → `x.grad` = 4.
- Computes `y2 = x**3`, calls `.backward()` again → `x.grad` becomes 4 + 12 = 16 (accumulated, not overwritten).
- Comment explicitly states "Gradients accumulate by default."

**Key Concepts**
- Gradient accumulation
- In-place `.grad` updates across multiple `.backward()` calls

**Q&A**
- Q: What is `x.grad` after both `backward()` calls? A: 16 (4 from y1 plus 12 from y2).
- Q: Does calling `.backward()` a second time reset `.grad`? A: No — gradients accumulate (add) by default unless explicitly zeroed.

## Cell 13 (code)
**Shows `x.grad.zero_()` and explains why optimizers call `zero_grad()`.**
- Manually zeroes accumulated gradients with `x.grad.zero_()`.
- Connects this directly to why PyTorch optimizers expose `optimizer.zero_grad()` before each training step.

**Key Concepts**
- `zero_grad()`
- Preventing unintended gradient accumulation across training steps

**Q&A**
- Q: What problem does `zero__()`/`zero_grad()` solve? A: It prevents gradients from earlier steps from accumulating into the current step's gradients.
- Q: Where in a real training loop would this be called? A: At the start of each iteration, before the forward pass and `loss.backward()`.

## Cell 14 (code)
**Vector-valued autograd example: gradient of y = W·x with respect to weight vector W.**
- Defines `W = torch.randn(3, requires_grad=True)` and a fixed input `x = [1.0, 2.0, 3.0]`.
- Computes `y = (W * x).sum()` and calls `y.backward()`.
- Prints `W.grad`, showing that `.backward()` only computes gradients and never changes tensor values.
- States the closed-form fact: if y = W·x, then ∂y/∂W = x.

**Key Concepts**
- Vector/multi-parameter autograd
- Dot-product gradient identity (∂(W·x)/∂W = x)
- `.backward()` does not mutate forward values

**Q&A**
- Q: What is `W.grad` equal to after this cell runs? A: The input vector `x` = [1.0, 2.0, 3.0], since y = W·x implies ∂y/∂W = x.
- Q: Does `backward()` change the values of `W` or `x`? A: No — it only populates `.grad`; parameter values are updated separately by an optimizer.

## Cell 15 (code)
**Demonstrates `.detach()` to break the autograd graph.**
- Creates `x = torch.tensor(2.0, requires_grad=True)`.
- Computes `y = x.detach() ** 2`, which produces a tensor that does **not** track gradients.
- Shows the printed values of `x` and `y` for comparison.

**Key Concepts**
- `.detach()`
- Excluding a computation from the autograd graph

**Q&A**
- Q: What does `.detach()` do here? A: It creates a new tensor sharing data with `x` but disconnected from the autograd graph, so operations on it won't track gradients.
- Q: Would `y.backward()` work after this? A: No — `y` would not require grad, since it was built from a detached tensor.

## Cell 16 (code)
**A single comment linking to the official PyTorch autograd tutorial.**
- Contains only a URL comment (`docs.pytorch.org/.../autograd_tutorial.html`) as a reference for further reading.
- No executable logic; serves as a citation/resource pointer.

**Key Concepts**
- External reference/documentation

**Q&A**
- Q: What does this final cell contain? A: Just a reference link (as a comment) to PyTorch's official autograd tutorial — no code logic.

---

## Notebook-Level Review

**Overall Summary**
This notebook builds intuition for PyTorch's autograd engine, starting from the motivation (manual derivatives don't scale) and progressing through leaf tensors, `requires_grad`, `grad_fn`, and the dynamically built computation graph. It demonstrates single- and multi-step chain-rule backpropagation (`y.backward()`, `z.backward()`), explains why deep learning relies on reverse-mode differentiation over forward-mode, and covers practical mechanics every PyTorch user needs: gradient accumulation across multiple `.backward()` calls, zeroing gradients with `.zero_()`/`zero_grad()`, vector-valued gradients (W·x), and using `.detach()` to exclude computations from the graph.

**Concept Map**
- *Foundations*: automatic differentiation, computation graphs, chain rule, manual vs. automatic differentiation
- *Autograd mechanics*: leaf tensors, `requires_grad`, `grad_fn`, dynamic graph construction, reverse-mode autodiff
- *Gradient lifecycle*: `.backward()`, gradient accumulation, `.zero_()` / `optimizer.zero_grad()`
- *Differentiation modes*: forward-mode vs. reverse-mode, why DL uses reverse-mode
- *Advanced tensor ops*: vector-valued gradients (dot product), `.detach()` to disconnect from the graph

**Mixed Q&A Quiz**
1. Q: Why does PyTorch use reverse-mode rather than forward-mode differentiation for training neural networks? A: Because models have millions of parameters but a single scalar loss output, and reverse-mode is efficient precisely for that many-inputs/one-output case (Cells 6, 9, 11).
2. Q: If you call `.backward()` twice on different loss expressions without zeroing gradients in between, what happens to `.grad`? A: The gradients accumulate (sum) rather than being overwritten, which is why training loops call `zero_grad()` first (Cells 12, 13).
3. Q: How does the ASCII graph in Cell 7 (`AddBackward0` → `PowBackward0`, `MulBackward0`) relate to the manual chain rule in Cell 10? A: Each backward-op node corresponds to a local derivative term; `dz/dx = dz/dy · dy/dx` is literally implemented by walking these `grad_fn` nodes in reverse (Cells 5-10).
4. Q: For y = W·x with W a vector, why is `W.grad` exactly equal to `x`? A: Because the partial derivative of a dot product with respect to one vector equals the other vector element-wise (∂(W·x)/∂W = x), demonstrated numerically in Cell 14.
5. Q: What is the practical difference between using `.detach()` and using `x.grad.zero_()`? A: `.detach()` prevents a specific tensor's operations from being tracked in the graph at all (no gradient will ever be computed for that branch), whereas `.zero_()` clears an already-computed gradient value so it doesn't accumulate into the next `.backward()` call (Cells 13, 15).
