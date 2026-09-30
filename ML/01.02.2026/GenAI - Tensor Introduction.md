# Notebook Study Summary: GenAI - Tensor Introduction

## Cell 1 (markdown) — Title: "Tensor Basics"

**Summary points**
- Introduces the notebook's topic: fundamentals of tensors in PyTorch.
- Sets the stage for hands-on exploration of tensor creation, operations, and use in simple neural networks.

**Key Concepts**
- Tensors as the core data structure in deep learning frameworks

**Q&A**
- Q: What is the purpose of this notebook? A: To teach the basics of tensors and their operations using PyTorch.

---

## Cell 2 (code) — Creating tensors and inspecting properties

**Summary points**
- Imports `torch` and creates a 1D tensor from a Python list using `torch.tensor()`.
- Creates a 2D tensor (matrix) and prints it to show nested-list-to-tensor conversion.
- Inspects tensor metadata via `.shape` and `.dtype` properties.
- Creates tensors pre-filled with zeros and ones using `torch.zeros()` and `torch.ones()` with specified shapes.

**Key Concepts**
- Tensor creation from Python lists
- Tensor dimensionality (1D vs 2D)
- Tensor shape and dtype attributes
- Initialization helpers (`zeros`, `ones`)

**Q&A**
- Q: What does `tensor_2d.shape` return? A: `torch.Size([2, 3])`, since it's a 2-row, 3-column matrix.
- Q: Why might you need `torch.zeros` or `torch.ones`? A: To initialize tensors (e.g., weights, biases, placeholders) with known default values before training.

---

## Cell 3 (code) — Element-wise tensor addition

**Summary points**
- Adds `tensor_1d` to another tensor of the same shape using the `+` operator.
- Demonstrates element-wise (not matrix) addition between two 1D tensors.

**Key Concepts**
- Element-wise arithmetic operations

**Q&A**
- Q: What operation does `+` perform on two tensors of the same shape? A: Element-wise addition, pairing up corresponding elements.

---

## Cell 4 (code) — Element-wise multiplication (scalar broadcast)

**Summary points**
- Multiplies `tensor_2d` by the scalar `2`, scaling every element.
- Demonstrates broadcasting a scalar across an entire tensor.

**Key Concepts**
- Broadcasting
- Scalar-tensor multiplication

**Q&A**
- Q: What happens to each element of `tensor_2d` in `tensor_2d * 2`? A: Each element is multiplied by 2 independently (element-wise scaling).

---

## Cell 5 (code) — Matrix multiplication

**Summary points**
- Defines two 2x2 matrices, `matrix_a` and `matrix_b`.
- Uses `torch.matmul()` to compute true matrix multiplication (not element-wise).
- Prints the resulting matrix product.

**Key Concepts**
- Matrix multiplication vs. element-wise multiplication
- `torch.matmul`

**Q&A**
- Q: How does `torch.matmul` differ from the `*` operator used in Cell 4? A: `matmul` performs linear-algebra matrix multiplication (row-by-column dot products), while `*` multiplies corresponding elements.
- Q: What would happen if `matrix_a` and `matrix_b` had incompatible shapes? A: `torch.matmul` would raise a runtime error due to shape mismatch.

---

## Cell 6 (code) — Reshaping tensors

**Summary points**
- Uses `.view(3, 2)` to reshape `tensor_2d` (originally 2x3) into a 3x2 tensor.
- Demonstrates that reshaping preserves the total number of elements while changing dimensional layout.

**Key Concepts**
- Tensor reshaping (`view`)
- Preservation of element count across reshape

**Q&A**
- Q: What is required for `.view()` to succeed? A: The new shape must have the same total number of elements as the original tensor, and the tensor must be contiguous in memory.

---

## Cell 7 (code) — Transposing tensors

**Summary points**
- Uses `.t()` to transpose `tensor_2d`, swapping rows and columns.
- Shows how transpose changes a 2x3 tensor into a 3x2 tensor by flipping axes (distinct from `.view` reshaping).

**Key Concepts**
- Tensor transpose

**Q&A**
- Q: How does transposing differ from reshaping with `.view()`? A: Transpose reorders existing data along swapped axes, while `.view()` just reinterprets the same data with a different shape without reordering.

---

## Cell 8 (code) — Indexing and slicing tensors

**Summary points**
- Prints the full `tensor_2d` for reference.
- Slices out the second column using `tensor_2d[:, 1]`.
- Iterates through all columns (`[:, 0]`, `[:, 1]`, `[:, 2]`) and all rows (`[0, :]`, `[1, :]`) individually.
- Highlights that tensor slicing syntax mirrors NumPy array slicing.

**Key Concepts**
- Tensor indexing/slicing
- Row vs. column selection
- Similarity to NumPy indexing

**Q&A**
- Q: What does `tensor_2d[:, 1]` return? A: The second column of the tensor (index 1), as a 1D tensor.
- Q: What does `tensor_2d[0, :]` return? A: The first row of the tensor.

---

## Cell 9 (code) — Stacking tensors

**Summary points**
- Uses `torch.stack()` to combine two copies of `tensor_1d` into a new tensor with an added dimension.
- Demonstrates how multiple 1D tensors can be combined into a 2D tensor.

**Key Concepts**
- Tensor stacking (`torch.stack`)
- Dimensionality increase when combining tensors

**Q&A**
- Q: What is the resulting shape after stacking two 1D tensors of length 5? A: A 2D tensor of shape `(2, 5)`.

---

## Cell 10 (code) — Converting tensor to NumPy array

**Summary points**
- Converts `tensor_1d` to a NumPy array using `.numpy()`.
- Shows interoperability between PyTorch tensors and NumPy arrays.

**Key Concepts**
- Tensor-to-NumPy conversion
- Framework interoperability

**Q&A**
- Q: What method converts a PyTorch tensor to a NumPy array? A: `.numpy()`.
- Q: What's a caveat of `.numpy()`? A: It only works on CPU tensors; the resulting array shares memory with the tensor, so changes to one affect the other.

---

## Cell 11 (code) — Moving tensors to GPU

**Summary points**
- Checks GPU availability with `torch.cuda.is_available()`.
- Moves `tensor_1d` to GPU using `.to('cuda')` if available; otherwise prints "NO GPU".
- Demonstrates conditional device placement, a common pattern in PyTorch code.

**Key Concepts**
- Device management (CPU vs. GPU)
- `torch.cuda.is_available()`
- `.to(device)` pattern

**Q&A**
- Q: Why check `torch.cuda.is_available()` before moving a tensor to GPU? A: To avoid errors on machines without a compatible GPU/CUDA installation.

---

## Cell 12 (code) — Performance comparison: list vs. NumPy vs. tensor, and broadcasting

**Summary points**
- Creates a large Python list, NumPy array, and PyTorch tensor of 1,000,000 elements.
- Times element-wise multiplication by 2 for each of the three data structures using `time.time()`.
- Prints timing results to compare performance (list comprehension is slowest, NumPy/tensor are much faster).
- Demonstrates broadcasting by adding a smaller array/tensor (`[1, 2, 3]`) to a 3x3 matrix of ones, in both NumPy and PyTorch.

**Key Concepts**
- Performance benchmarking with `time.time()`
- Vectorized operations vs. Python loops
- Broadcasting (adding a smaller shape to a larger one)

**Q&A**
- Q: Why is the Python list operation slowest? A: List comprehensions perform the multiplication in pure Python with per-element overhead, while NumPy/PyTorch use optimized, vectorized C/C++ backends.
- Q: What does broadcasting allow in `torch_matrix + torch.tensor([1, 2, 3])`? A: It allows a smaller tensor (shape `(3,)`) to be automatically expanded and added to each row of the larger `(3, 3)` tensor without manual replication.

---

## Cell 13 (code) — GPU acceleration timing

**Summary points**
- Conditionally creates a tensor directly on the GPU (`device='cuda'`) if available.
- Times a multiplication operation on the GPU tensor.
- Prints "GPU not available" as a fallback when no CUDA device exists (as in this run, based on the notebook's structure).

**Key Concepts**
- GPU tensor creation
- Timing GPU operations
- Device-aware code branching

**Q&A**
- Q: What is the benefit of creating a tensor directly with `device=device` versus creating it on CPU and moving it later? A: It avoids an extra CPU-to-GPU memory transfer step, creating the tensor directly in GPU memory.

---

## Cell 14 (markdown) — Section header: "Some sample cases where we will use these Tensor."

**Summary points**
- Marks a transition from basic tensor mechanics to applied examples (neurons, neural networks).
- Notes that deeper coverage of these examples will come in future classes.

**Key Concepts**
- Transition from fundamentals to applied deep learning examples

**Q&A**
- Q: What is the purpose of this section header? A: To signal a shift from tensor operation basics to practical examples like neurons and neural networks.

---

## Cell 15 (code) — Empty cell

**Summary points**
- This cell contains no code or output.

---

## Cell 16 (code) — Simulating a simple artificial neuron

**Summary points**
- Defines `simple_neuron()`, a function that computes a single neuron's output from `inputs`, `weights`, and `bias`.
- Converts Python lists to `float32` tensors using `torch.tensor(..., dtype=torch.float32)`.
- Computes the weighted sum via `torch.dot(inputs, weights) + bias`.
- Applies the sigmoid activation function (`torch.sigmoid`) to produce a bounded output between 0 and 1.
- Runs the function on example inputs/weights/bias and prints the result.

**Key Concepts**
- Artificial neuron computation (weighted sum + activation)
- Dot product (`torch.dot`)
- Sigmoid activation function
- `dtype` specification for tensors

**Q&A**
- Q: What does `torch.dot(inputs, weights)` compute? A: The dot product — the sum of element-wise products of the two 1D tensors.
- Q: Why apply `torch.sigmoid` to the result? A: To squash the linear output into a range between 0 and 1, mimicking a neuron's activation/firing behavior.
- Q: What would happen if `inputs` and `weights` had different lengths? A: `torch.dot` would raise an error since dot product requires equal-length vectors.

---

## Cell 17 (code) — Empty cell

**Summary points**
- This cell contains no code or output.

---

## Cell 18 (code) — Training a simple neural network (`SimpleNN`) on synthetic data

**Summary points**
- Generates synthetic data: 100 samples with 3 features (`X`) and binary labels (`y`).
- Defines `SimpleNN`, a two-layer network (`fc1`: 3→5, `fc2`: 5→1) using `nn.Module`, with `relu` and `sigmoid` activations.
- Sets up `BCELoss` (binary cross-entropy) and `SGD` optimizer.
- Runs a training loop for 5 epochs: forward pass, loss computation, backward pass, optimizer step, and gradient reset.
- Demonstrates batch processing by iterating over the dataset in chunks of size 10 and printing batch outputs.

**Key Concepts**
- `nn.Module` subclassing for custom models
- Forward pass and activation functions (ReLU, Sigmoid)
- Binary Cross-Entropy Loss (`nn.BCELoss`)
- Stochastic Gradient Descent (`optim.SGD`)
- Training loop mechanics: `loss.backward()`, `optimizer.step()`, `optimizer.zero_grad()`
- Batch processing

**Q&A**
- Q: Why is `optimizer.zero_grad()` called each epoch? A: To clear gradients from the previous iteration so they don't accumulate incorrectly with the current gradients.
- Q: Why use `sigmoid` on the final layer output here? A: Because the task is binary classification, and `BCELoss` expects probabilities in [0, 1].
- Q: What would happen if the training loop ran for many more epochs? A: Loss would typically continue decreasing (up to a point) as the model fits the synthetic data, though with random labels here, it may plateau since there's no real signal to learn from.

---

## Cell 19 (code) — Regression model on California Housing dataset

**Summary points**
- Loads the California Housing dataset via `sklearn.datasets.fetch_california_housing()`.
- Standardizes features with `StandardScaler` and reshapes target `y` into a column vector.
- Splits data into train/test sets using `train_test_split`.
- Converts NumPy arrays to `float32` PyTorch tensors.
- Defines `RegressionNN`, a two-layer network (input→10→1) with ReLU activation on the hidden layer and linear output (for regression).
- Trains for 20 epochs using `MSELoss` and `SGD`, printing loss each epoch.
- Evaluates the trained model on the test set and prints test MSE.

**Key Concepts**
- Real-world dataset loading (`fetch_california_housing`)
- Feature standardization (`StandardScaler`)
- Train/test split (`train_test_split`)
- Regression neural network architecture (linear output, no final activation)
- Mean Squared Error loss (`nn.MSELoss`)
- Model evaluation mode (`model.eval()`) and `.detach()`

**Q&A**
- Q: Why is there no activation function after `fc2` in `RegressionNN`? A: Because this is a regression task requiring unbounded continuous output, unlike classification which needs a bounded probability via sigmoid.
- Q: Why standardize the features before training? A: To ensure all features are on a similar scale, which helps gradient-based optimization converge more reliably and quickly.
- Q: What is the purpose of `model.eval()` before evaluation? A: It switches the model to evaluation mode, disabling training-specific behaviors like dropout or batch norm updates (though none are used here, it's still good practice).

---

## Cell 20 (code) — Empty cell

**Summary points**
- This cell contains no code or output.

---

## Notebook-Level Review

**Overall Summary**
This notebook is an introductory tour of PyTorch tensors, starting with tensor creation, properties, and core operations (arithmetic, matrix multiplication, reshaping, transposing, slicing, stacking, and NumPy conversion). It then covers device management (CPU vs. GPU) and benchmarks performance differences between plain Python lists, NumPy arrays, and PyTorch tensors, including a demonstration of broadcasting. The second half applies these tensor fundamentals to practical deep learning examples: a hand-computed artificial neuron using dot products and sigmoid activation, a simple binary classification neural network trained on synthetic data, and a regression neural network trained on the real-world California Housing dataset. Together, the notebook builds a foundation from raw tensor mechanics up to end-to-end model training and evaluation workflows in PyTorch.

**Concept Map**
- *Tensor Fundamentals*: creation, shape/dtype, zeros/ones, reshaping (`view`), transpose, indexing/slicing, stacking
- *Tensor Operations*: element-wise arithmetic, matrix multiplication (`matmul`), broadcasting
- *Interoperability & Devices*: NumPy conversion, GPU placement (`cuda`), performance benchmarking
- *Neural Network Building Blocks*: dot product-based neuron, `nn.Module`, activation functions (ReLU, Sigmoid)
- *Training Workflow*: loss functions (`BCELoss`, `MSELoss`), optimizers (`SGD`), training loop (forward/backward/step/zero_grad), batching
- *Applied Modeling*: synthetic binary classification, real-world regression (California Housing), data preprocessing (`StandardScaler`, `train_test_split`), evaluation (`model.eval()`, MSE)

**Mixed Q&A Quiz**
1. Q: How does the tensor creation and reshaping covered early in the notebook (Cells 2, 6, 7) relate to building the neural network layers later (Cells 18-19)? A: Understanding tensor shapes and reshaping is essential because neural network layers (`nn.Linear`) require inputs and weights to have compatible shapes, and reshaping/transposing operations are used internally during matrix multiplications in forward passes.
2. Q: Both Cell 18 (`SimpleNN`) and Cell 19 (`RegressionNN`) follow the same training loop pattern. What are the four key steps repeated in each epoch, and why does the order matter? A: (1) forward pass to compute outputs, (2) compute loss, (3) `loss.backward()` to compute gradients, (4) `optimizer.step()` to update weights, followed by `optimizer.zero_grad()`. Order matters because gradients must be computed before the optimizer can use them to update weights, and gradients must be cleared to prevent accumulation across epochs.
3. Q: How does the broadcasting demonstrated in Cell 12 relate to operations inside `SimpleNN` and `RegressionNN`? A: Broadcasting allows operations like adding a bias vector to a batch of outputs (as happens internally in `nn.Linear`) without manually resizing tensors, the same mechanism shown when adding a `(3,)` tensor to a `(3,3)` matrix.
4. Q: Why does the classification network (Cell 18) use `sigmoid` + `BCELoss` while the regression network (Cell 19) uses a linear output + `MSELoss`? A: Classification requires bounded probability outputs (0 to 1) compared against binary labels, best matched with `BCELoss`, whereas regression predicts continuous unbounded values, so no activation is applied on the output and `MSELoss` measures squared numeric error.
5. Q: Based on the performance comparison in Cell 12 and the GPU checks in Cells 11 and 13, why might a practitioner still choose to run small experiments (like the neuron in Cell 16) on CPU rather than GPU? A: For small tensors/operations, the overhead of transferring data to the GPU and launching GPU kernels can outweigh the speedup, making CPU execution just as fast or faster; GPU benefits become significant mainly at scale (large tensors/batches), as illustrated by the large-scale benchmarks in Cell 12.
