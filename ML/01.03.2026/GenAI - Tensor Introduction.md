# Notebook Summary: GenAI - Tensor Introduction.ipynb

## Cell 1 (markdown) — Section header: Tensor Basics
**Summary points**
- Markdown header introducing the "Tensor Basics" section of the notebook.
- Sets up the topic: PyTorch tensors as the fundamental data structure for GenAI/ML work.

**Key Concepts**
- Notebook section organization
- Tensors (introduced as a topic)

**Q&A**
- Q: What topic does this header introduce? A: Basics of PyTorch tensors.

## Cell 2 (code) — Creating tensors and inspecting properties
**Summary points**
- Imports `torch` and creates a 1D tensor (`tensor_1d`) from a Python list of integers.
- Creates a 2D tensor (`tensor_2d`) representing a 2x3 matrix from a nested list.
- Inspects tensor metadata: `.shape` (dimensions) and `.dtype` (data type) of `tensor_2d`.
- Creates a 3x3 tensor of zeros (`torch.zeros`) and a 2x4 tensor of ones (`torch.ones`) to show tensor initialization helpers.

**Key Concepts**
- `torch.tensor`
- Tensor shape and dtype
- `torch.zeros`, `torch.ones`

**Q&A**
- Q: What is `tensor_2d.shape`? A: `torch.Size([2, 3])`.
- Q: What's the difference between `torch.zeros((3,3))` and `torch.ones((2,4))`? A: They create tensors of different shapes (3x3 vs 2x4) filled with `0`s and `1`s respectively.

## Cell 3 (code) — Element-wise tensor addition
**Summary points**
- Adds `tensor_1d` (`[1,2,3,4,5]`) to a new tensor `[5,4,3,2,1]` element-wise using `+`.
- Prints the resulting sum tensor.
- Demonstrates that arithmetic operators on tensors operate element-wise, similar to NumPy arrays.

**Key Concepts**
- Element-wise tensor addition

**Q&A**
- Q: What is the result of `tensor_1d + torch.tensor([5,4,3,2,1])`? A: `tensor([6, 6, 6, 6, 6])`.
- Q: What would happen if the two tensors had different shapes and couldn't broadcast? A: PyTorch would raise a runtime error about incompatible shapes for element-wise operations.

## Cell 4 (code) — Element-wise scalar multiplication
**Summary points**
- Multiplies `tensor_2d` by scalar `2` using `tensor_2d * 2`.
- Prints the resulting matrix, where every element is doubled.
- Demonstrates broadcasting a scalar across all elements of a tensor.

**Key Concepts**
- Element-wise multiplication
- Scalar broadcasting

**Q&A**
- Q: What is the result of `tensor_2d * 2` given `tensor_2d = [[1,2,3],[4,5,6]]`? A: `[[2,4,6],[8,10,12]]`.
- Q: Is this the same as matrix multiplication? A: No — this is element-wise scalar multiplication, not matrix (`matmul`) multiplication.

## Cell 5 (code) — Matrix multiplication with `torch.matmul`
**Summary points**
- Creates two 2x2 tensors, `matrix_a` and `matrix_b`.
- Computes their matrix product using `torch.matmul(matrix_a, matrix_b)`.
- Prints the resulting 2x2 matrix, demonstrating true (linear-algebra) matrix multiplication as opposed to element-wise multiplication from Cell 4.

**Key Concepts**
- `torch.matmul`
- Matrix multiplication (linear algebra)

**Q&A**
- Q: What is `torch.matmul([[1,2],[3,4]], [[5,6],[7,8]])`? A: `[[19, 22], [43, 50]]`.
- Q: How does `matmul` differ from the `*` operator used in Cell 4? A: `*` multiplies elements position-by-position, while `matmul` performs true matrix multiplication (dot products of rows and columns).

## Cell 6 (code) — Reshaping tensors with `.view`
**Summary points**
- Reshapes `tensor_2d` (originally 2x3) into a 3x2 tensor using `.view(3, 2)`.
- Prints the reshaped tensor, showing the same data rearranged into new dimensions.
- Demonstrates that `.view` changes shape without copying/altering the underlying data (must be compatible in total element count).

**Key Concepts**
- Tensor reshaping (`.view`)
- Shape compatibility (total elements preserved)

**Q&A**
- Q: What is `tensor_2d.view(3, 2)` given the original 2x3 tensor `[[1,2,3],[4,5,6]]`? A: `[[1,2],[3,4],[5,6]]`.
- Q: Why must the new shape have the same total number of elements as the original? A: `.view` reinterprets the same underlying memory, so it can't change the total element count — only how they're arranged.

## Cell 7 (code) — Transposing tensors with `.t()`
**Summary points**
- Transposes `tensor_2d` using `.t()`, swapping rows and columns.
- Prints the transposed tensor, converting the 2x3 matrix into a 3x2 matrix.
- Demonstrates a common linear-algebra operation available directly as a tensor method.

**Key Concepts**
- Tensor transpose (`.t()`)

**Q&A**
- Q: What is `tensor_2d.t()` given `[[1,2,3],[4,5,6]]`? A: `[[1,4],[2,5],[3,6]]`.
- Q: Is `.t()` the same operation as `.view(3,2)` from Cell 6? A: No — `.t()` transposes (swaps rows/columns based on values), while `.view` just reshapes the same sequential data into new dimensions; the results are only coincidentally similar in shape, not content.

## Cell 8 (code) — Slicing and indexing tensors
**Summary points**
- Prints `tensor_2d` for reference, then slices the second column using `tensor_2d[:, 1]`.
- Iterates through each column (`[:, 0]`, `[:, 1]`, `[:, 2]`) and each row (`[0, :]`, `[1, :]`) individually, printing each.
- Demonstrates NumPy-style slicing syntax (`[rows, columns]`) applies directly to PyTorch tensors.
- Comment explicitly notes the similarity to NumPy array indexing.

**Key Concepts**
- Tensor slicing/indexing
- NumPy-style indexing conventions

**Q&A**
- Q: What does `tensor_2d[:, 1]` return? A: `tensor([2, 5])` — the second column.
- Q: What does `tensor_2d[0, :]` return? A: `tensor([1, 2, 3])` — the first row.

## Cell 9 (code) — Stacking tensors with `torch.stack`
**Summary points**
- Stacks `tensor_1d` with itself using `torch.stack((tensor_1d, tensor_1d))`.
- Prints the resulting 2D tensor, which combines two 1D tensors into a new dimension.
- Demonstrates how to combine multiple tensors of the same shape into a higher-dimensional tensor.

**Key Concepts**
- `torch.stack`
- Dimensionality increase when combining tensors

**Q&A**
- Q: What is the shape of the result of `torch.stack((tensor_1d, tensor_1d))` if `tensor_1d` has shape `(5,)`? A: `(2, 5)`.
- Q: How does `torch.stack` differ from concatenation (`torch.cat`)? A: `stack` creates a new dimension to combine tensors, while `cat` joins tensors along an existing dimension without adding a new one.

## Cell 10 (code) — Converting a tensor to a NumPy array
**Summary points**
- Converts `tensor_1d` to a NumPy array using `.numpy()`.
- Prints the resulting NumPy array, demonstrating interoperability between PyTorch and NumPy.
- Note: this reuses the variable name `numpy_array`, which is later overwritten again in Cell 12 for a different purpose.

**Key Concepts**
- Tensor-to-NumPy conversion (`.numpy()`)
- PyTorch/NumPy interoperability

**Q&A**
- Q: What does `tensor_1d.numpy()` return? A: A NumPy array with the same values as `tensor_1d`, e.g. `array([1, 2, 3, 4, 5])`.
- Q: Does `.numpy()` work on GPU tensors directly? A: No — a tensor must be on the CPU before calling `.numpy()`; GPU tensors need to be moved to CPU first (e.g., via `.cpu()`).

## Cell 11 (code) — Moving tensors to GPU conditionally
**Summary points**
- Checks `torch.cuda.is_available()` to determine if a GPU is present.
- If available, moves `tensor_1d` to the GPU using `.to('cuda')`.
- If not available, prints `'NO GPU'` instead, showing defensive/conditional device handling.
- Demonstrates the standard pattern for writing device-agnostic PyTorch code.

**Key Concepts**
- GPU acceleration (`torch.cuda.is_available()`, `.to('cuda')`)
- Device-agnostic code patterns

**Q&A**
- Q: What is printed if no GPU is available? A: `NO GPU`.
- Q: Why check `torch.cuda.is_available()` before calling `.to('cuda')`? A: To avoid runtime errors on machines without a CUDA-capable GPU.

## Cell 12 (code) — Performance comparison: Python list vs. NumPy vs. PyTorch
**Summary points**
- Creates a large list of 1,000,000 integers, then converts it to both a NumPy array and a PyTorch tensor.
- Times element-wise multiplication by 2 for all three representations (Python list comprehension, NumPy array, PyTorch tensor) using `time.time()`.
- Demonstrates broadcasting by adding a smaller array/tensor (`[1,2,3]`) to a `(3,3)` matrix of ones for both NumPy and PyTorch.
- Shows (via comments with sample output) that NumPy/PyTorch vectorized operations are dramatically faster than plain Python loops.

**Key Concepts**
- Performance benchmarking (`time.time()`)
- Vectorized operations vs. Python loops
- Broadcasting (NumPy and PyTorch)

**Q&A**
- Q: Based on the sample output in the comments, which was fastest: Python list, NumPy array, or PyTorch tensor (on CPU)? A: NumPy array was fastest in the sample output (~0.02s), followed by PyTorch tensor (~0.09s), with the plain Python list slowest (~0.45s).
- Q: What does the broadcasting example demonstrate? A: Adding a 1D array/tensor `[1,2,3]` to a `(3,3)` matrix automatically applies the addition to each row, without manually looping.

## Cell 13 (code) — GPU acceleration timing (conditional)
**Summary points**
- Checks `torch.cuda.is_available()` again, this time to benchmark tensor multiplication specifically on the GPU.
- If available, creates `torch_tensor_gpu` directly on the `'cuda'` device and times a `* 2` operation on it.
- If not available, prints `"GPU not available"` instead, gracefully skipping the benchmark.
- Extends the CPU-based performance comparison from Cell 12 to include a GPU benchmark when hardware supports it.

**Key Concepts**
- GPU-based tensor computation timing
- Conditional hardware-dependent code execution

**Q&A**
- Q: What determines whether this cell prints a timing result or "GPU not available"? A: Whether `torch.cuda.is_available()` returns `True` (i.e., whether a CUDA GPU is present on the machine).
- Q: Why would GPU computation typically be faster than CPU for large tensors? A: GPUs have many parallel cores that can perform element-wise operations on large batches of data simultaneously, unlike a CPU which processes more sequentially.

## Cell 14 (markdown) — Section header: Sample use cases
**Summary points**
- Markdown header transitioning to practical/applied examples of tensors.
- Notes that these are just introductory examples, with deeper coverage planned for future classes.

**Key Concepts**
- Notebook section organization

**Q&A**
- Q: What does this header signal about the upcoming cells? A: That simplified example use-cases of tensors (e.g., neural network basics) follow, with more detail to come in future lessons.

## Cell 15 (code) — Empty cell
**Summary points**
- Empty code cell with no content; likely a spacer before the neuron example.

**Key Concepts**
- N/A (no content)

**Q&A**
- Q: What does this cell do? A: Nothing — it is empty.

## Cell 16 (code) — Simulating a single artificial neuron
**Summary points**
- Defines `simple_neuron(inputs, weights, bias)` which converts Python lists to `float32` tensors.
- Computes the weighted sum `z = torch.dot(inputs, weights) + bias`, mimicking a single neuron's linear combination step.
- Applies the sigmoid activation function (`torch.sigmoid(z)`) to squash the output into a `(0,1)` range.
- Runs the neuron with example `inputs`, `weights`, and `bias`, printing the final output — introducing core building blocks of neural networks (weights, bias, activation).

**Key Concepts**
- Weighted sum (dot product) of inputs and weights
- Bias term
- Sigmoid activation function
- Basic neuron/perceptron model

**Q&A**
- Q: What mathematical operation does `torch.dot(inputs, weights) + bias` represent? A: The weighted sum of inputs plus a bias term — the linear part of a neuron's computation, i.e. $z = \sum_i (x_i \cdot w_i) + b$.
- Q: Why is `torch.sigmoid` applied to `z`? A: To convert the raw linear output into a bounded value between 0 and 1, mimicking a neuron's activation/output.

## Cell 17 (code) — Empty cell
**Summary points**
- Empty code cell with no content; likely a spacer before the full neural network example.

**Key Concepts**
- N/A (no content)

**Q&A**
- Q: What does this cell do? A: Nothing — it is empty.

## Cell 18 (code) — Simple binary classification neural network (`SimpleNN`)
**Summary points**
- Generates synthetic data: `X` (100 samples, 3 features, random) and `y` (100 binary labels, random 0/1).
- Defines `SimpleNN(nn.Module)` with two linear layers (`fc1`: 3→5, `fc2`: 5→1), using `relu` then `sigmoid` activations in `forward`.
- Sets up `BCELoss` (binary cross-entropy) as the criterion and `SGD` as the optimizer, then trains for 5 epochs using the standard forward → loss → backward → step → zero_grad loop.
- After training, demonstrates batch processing by iterating over `X`/`y` in chunks of size 10 and printing each batch's model output.

**Key Concepts**
- `torch.nn.Module` / custom neural network class
- Linear layers (`nn.Linear`), ReLU, Sigmoid activations
- Binary Cross-Entropy Loss (`nn.BCELoss`)
- SGD optimizer (`optim.SGD`)
- Training loop (forward/backward/step/zero_grad)
- Batch processing

**Q&A**
- Q: What do `fc1` and `fc2` represent in `SimpleNN`? A: `fc1` is a linear layer mapping 3 input features to 5 hidden units; `fc2` maps those 5 hidden units to 1 output value.
- Q: Why is `sigmoid` used on the final output but `relu` on the hidden layer? A: `sigmoid` squashes the output into (0,1), suitable for binary classification probabilities, while `relu` is a common non-linearity for hidden layers that helps with gradient flow.
- Q: What does `optimizer.zero_grad()` do, and why is it needed each epoch? A: It resets accumulated gradients from the previous step to zero, preventing gradients from earlier iterations from incorrectly accumulating into the current update.

## Cell 19 (code) — Regression neural network on California Housing dataset
**Summary points**
- Loads the California Housing dataset via `sklearn.datasets.fetch_california_housing`, extracting features `X` and target `y`.
- Standardizes features with `StandardScaler` and reshapes `y` into a column vector; splits into train/test sets using `train_test_split`.
- Converts all data splits into `float32` PyTorch tensors, then defines `RegressionNN(nn.Module)` with two linear layers (`fc1`: input→10, `fc2`: 10→1) using `relu` in the hidden layer and no activation on the output (regression, not classification).
- Trains for 20 epochs using `MSELoss` and `SGD`, then evaluates on the test set with `model.eval()` and reports the test MSE — contrasting a real regression task with the earlier binary classification toy example.

**Key Concepts**
- Real-world dataset loading (`fetch_california_housing`)
- Feature standardization (`StandardScaler`)
- Train/test split (`train_test_split`)
- Regression neural network (no output activation)
- Mean Squared Error Loss (`nn.MSELoss`)
- Model evaluation mode (`model.eval()`) vs. training mode (`model.train()`)

**Q&A**
- Q: Why is there no activation function after `fc2` in `RegressionNN`, unlike `SimpleNN` in Cell 18? A: Because this is a regression task predicting continuous housing prices, not bounded probabilities, so an unrestricted linear output is appropriate.
- Q: Why is `StandardScaler` applied to `X` before training? A: To normalize feature scales (zero mean, unit variance), which typically helps gradient-based optimization converge more reliably.
- Q: What is the purpose of calling `model.eval()` before generating `y_pred`? A: To switch the model into evaluation mode, which disables training-specific behaviors (e.g., dropout/batch norm updates) for consistent inference — though this particular model doesn't use dropout/batchnorm, it's still best practice.

## Cell 20 (code) — Empty cell
**Summary points**
- Empty code cell with no content; final trailing placeholder in the notebook.

**Key Concepts**
- N/A (no content)

**Q&A**
- Q: What does this cell do? A: Nothing — it is empty.

## Notebook-Level Review

**Overall Summary**
This notebook introduces PyTorch tensors as the foundational data structure for deep learning, starting with tensor creation, properties (shape, dtype), and basic arithmetic (element-wise operations, matrix multiplication). It then covers structural operations like reshaping, transposing, slicing, and stacking, followed by interoperability with NumPy and device management (CPU vs. GPU). A performance comparison demonstrates why vectorized tensor operations (NumPy/PyTorch) vastly outperform plain Python loops, with an optional GPU benchmark. The notebook culminates in applied examples: a single artificial neuron with a sigmoid activation, a small binary classification neural network (`SimpleNN`) trained on synthetic data, and a regression neural network (`RegressionNN`) trained on the real California Housing dataset — together illustrating the full pipeline from raw tensors to a trained/evaluated neural network.

**Concept Map**
- **Tensor Fundamentals**: creation (`torch.tensor`, `zeros`, `ones`), shape/dtype inspection.
- **Tensor Operations**: element-wise arithmetic, `matmul`, reshaping (`.view`), transpose (`.t()`), slicing/indexing, stacking (`torch.stack`).
- **Interoperability & Devices**: NumPy conversion (`.numpy()`), GPU transfer (`.to('cuda')`), `torch.cuda.is_available()`.
- **Performance**: benchmarking Python lists vs. NumPy vs. PyTorch tensors (CPU and GPU), broadcasting.
- **Neural Network Building Blocks**: weighted sum/dot product, bias, sigmoid activation, single neuron simulation.
- **Neural Network Training (Classification)**: `nn.Module`, `nn.Linear`, `relu`/`sigmoid`, `BCELoss`, `SGD`, training loop, batch processing.
- **Neural Network Training (Regression)**: real dataset loading, `StandardScaler`, train/test split, `MSELoss`, `model.eval()` vs. `model.train()`.

**Mixed Q&A Quiz**
- Q: How does the broadcasting example in Cell 12 relate to the matrix operations in Cells 4–5? A: Cell 4 shows scalar broadcasting (multiplying every element by 2), Cell 5 shows true matrix multiplication (`matmul`), and Cell 12 extends the idea by broadcasting a smaller 1D array/tensor across a 2D matrix during addition — all are variations of applying operations across tensor shapes without explicit loops.
- Q: Both `SimpleNN` (Cell 18) and `RegressionNN` (Cell 19) use `relu` in a hidden layer, but differ in their output layer activation. Why? A: `SimpleNN` is doing binary classification, so it applies `sigmoid` to the output to get a probability in (0,1); `RegressionNN` predicts continuous housing prices, so it leaves the output unactivated (linear) since values aren't bounded.
- Q: How does the single neuron simulation in Cell 16 conceptually connect to the `nn.Linear` layers used in Cells 18–19? A: Cell 16 manually computes what a single `nn.Linear` unit does internally — a weighted sum plus bias, followed by an activation function — while `nn.Linear` layers in Cells 18–19 automate this computation (and its learnable parameters) for many neurons at once.
- Q: Why does the notebook convert data to NumPy (Cell 10) and also test GPU availability (Cells 11, 13) before diving into neural networks? A: To establish PyTorch's interoperability with the wider Python data science ecosystem (NumPy) and its hardware flexibility (CPU/GPU), both of which are essential for practical deep learning workflows introduced later in the notebook.
- Q: What is the key structural difference between the training loops in Cell 18 and Cell 19 regarding evaluation? A: Cell 18 doesn't explicitly call `model.eval()` after training (it just runs batches), while Cell 19 explicitly switches to `model.eval()` before generating test predictions and computing test MSE, reflecting proper train/test separation for a real dataset.
