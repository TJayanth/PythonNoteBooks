# CNN Architecture Design.ipynb — Notebook Summary

## Cell 1 (markdown)
**Title: "Let's design and train a CNN from scratch to classify images."**
- Sets the overall goal of the notebook: build CNN architectures incrementally and apply them to real classification tasks.
- Frames the progression from a simple grayscale-image CNN to a deeper RGB CNN and finally a real dataset (Flowers102).

**Key Concepts**
- CNN design progression (simple → deep)
- Image classification

**Q&A**
- Q: What is the notebook's stated goal? A: To design and train a CNN from scratch for image classification.

## Cell 2 (code)
**Defines `MyFirstCNN`: a minimal 1-conv-layer CNN for grayscale (e.g., MNIST-like) images.**
- `nn.Conv2d(1, 8, 3)` takes 1 input channel, produces 8 feature maps, 3×3 kernel, no padding.
- `nn.MaxPool2d(2, 2)` halves spatial dimensions.
- `nn.Linear(8*13*13, 10)` maps flattened features to 10 output classes.
- `forward()` applies conv → pool → ReLU → flatten → fully connected.
- Library/API: `torch.nn` (`nn.Module`, `nn.Conv2d`, `nn.MaxPool2d`, `nn.Linear`), `torch.nn.functional.relu`.

**Key Concepts**
- `nn.Conv2d` (in/out channels, kernel size)
- `nn.MaxPool2d`
- Flattening (`x.view`)
- Fully connected classification head

**Q&A**
- Q: How many output classes does `MyFirstCNN` produce? A: 10.
- Q: What is the spatial size after `conv1` and after `pool` for a 28×28 input? A: 26×26 after conv1 (no padding, 3×3 kernel), then 13×13 after 2×2 max pooling.

## Cell 3 (code — explanatory text, not runnable)
**Step-by-step shape trace explaining `MyFirstCNN`'s layers and forward pass.**
- Explains `Conv2d(1,8,3)`: 1 input channel, 8 filters, 3×3 kernel; no-padding output size formula (28−3+1=26).
- Explains `MaxPool2d(2,2)`: halves 26×26 → 13×13.
- Explains `Linear(8*13*13, 10)`: flattens 1352 features into 10 class scores.
- Walks through the `forward()` method line by line, including why `x.view(-1, ...)` is used to flatten while auto-inferring batch size.

**Key Concepts**
- Convolution output-size formula (no padding): output = input − kernel + 1
- Flattening for FC layers
- `-1` in `view()` for automatic batch-size inference

**Q&A**
- Q: What is the output shape after `self.conv1(x)` for a [1,28,28] input? A: [8, 26, 26].
- Q: Why does `x.view(-1, 8*13*13)` use -1 as the first argument? A: So PyTorch automatically infers the batch size dimension instead of hardcoding it.
- Q: What does adding `F.relu` accomplish in the forward pass? A: It introduces non-linearity after the conv+pool block, which is essential for the network to learn non-linear decision boundaries.

## Cell 4 (code)
**Defines `MyCIFARCNN`: a deeper 2-conv-layer CNN for CIFAR-like RGB images (10 classes).**
- Uses `nn.Conv2d(3, 16, 3, padding=1)` then `nn.Conv2d(16, 32, 3, padding=1)`, each followed by the same shared `MaxPool2d(2,2)`.
- Adds two fully connected layers: `fc1` (32*8*8 → 64) and `fc2` (64 → 10).
- `forward()` chains conv1→pool→ReLU, conv2→pool→ReLU, flatten, fc1→ReLU, fc2.
- Library/API: `torch.nn`, `torch.nn.functional`.

**Key Concepts**
- Padding to preserve spatial size (`padding=1`)
- Stacking multiple conv+pool blocks
- Two-layer fully connected classification head

**Q&A**
- Q: Why is `padding=1` used in the conv layers here but not in `MyFirstCNN`? A: To preserve the input's spatial dimensions (32×32) after convolution, instead of shrinking them.
- Q: What is the flattened feature size before `fc1`? A: 32 × 8 × 8 = 2048.

## Cell 5 (code — explanatory text, not runnable)
**Detailed shape trace for `MyCIFARCNN`, layer by layer.**
- `conv1` with padding=1 keeps [3,32,32] → [16,32,32]; pooling → [16,16,16].
- `conv2` with padding=1: [16,16,16] → [32,16,16]; pooling → [32,8,8].
- `fc1`: flattens 32×8×8=2048 features → 64 hidden neurons ("acts as a hidden layer").
- `fc2`: final classification layer, 64 → 10 class scores.
- Restates the full `forward()` method with inline shape comments.

**Key Concepts**
- Effect of padding on convolution output size
- Successive downsampling via pooling
- Hidden dense layer vs. output layer roles

**Q&A**
- Q: What is the output shape after the second `MaxPool2d` call? A: [32, 8, 8].
- Q: What does `fc1` (32*8*8 → 64) contribute that a single `fc2` alone would not? A: It adds a learnable hidden representation/non-linearity, increasing the model's capacity to combine convolutional features before final classification.

## Cell 6 (code — summary table, not runnable)
**Table summarizing every layer's output shape and purpose for `MyCIFARCNN`.**
- Tabulates Input → Conv1 → Pool1 → Conv2 → Pool2 → Flatten → FC1 → FC2 with shapes and brief explanations.
- Acts as a consolidated reference/cheat-sheet after the two previous explanatory cells.

**Key Concepts**
- End-to-end shape bookkeeping for a CNN

**Q&A**
- Q: According to the table, what is the output shape after Conv2? A: [32, 16, 16].
- Q: What is the final output shape of the network (FC2)? A: [10] — logits for 10 classes.

## Cell 7 (raw)
**Design brief for a deeper 3-conv-layer CNN with 2 FC layers.**
- Specifies the architecture to build next: Conv1 (→16, 3×3, ReLU, MaxPool), Conv2 (16→32), Conv3 (32→64), each followed by pooling.
- Then Flatten → FC1 (128 neurons, ReLU) → FC2 (10 outputs).
- Acts as a design spec preceding the `CNN3Layer` implementation in the next cell.

**Key Concepts**
- Architecture planning/specification before implementation

**Q&A**
- Q: How many convolutional layers does this new design specify? A: 3 (16, 32, then 64 filters).
- Q: What is the final FC layer's output size, and why? A: 10, matching the number of target classes.

## Cell 8 (code)
**Implements `CNN3Layer`: a 3-conv-layer CNN matching the Cell 7 design spec.**
- `conv1` (3→16), `conv2` (16→32), `conv3` (32→64), all 3×3 with `padding=1`, each followed by the shared `MaxPool2d(2,2)`.
- `fc1`: `64*4*4 → 128` (after 3 poolings: 32→16→8→4); `fc2`: `128 → 10`.
- `forward()` applies pool(ReLU(conv)) three times, flattens to 1024 features, then FC1→ReLU→FC2.
- Library/API: `torch`, `torch.nn`, `torch.nn.functional`.

**Key Concepts**
- Deeper CNN stacking (3 conv blocks)
- Progressive channel increase (16→32→64) paired with progressive spatial shrinkage (32→16→8→4)

**Q&A**
- Q: What is the flattened feature size fed into `fc1`? A: 64 × 4 × 4 = 1024.
- Q: How does the spatial size shrink from input to the final pooled feature map? A: 32 → 16 (after pool1) → 8 (after pool2) → 4 (after pool3).

## Cell 9 (code — section marker)
**Comment marking the transition to a real dataset: Flowers102 classification.**
- Signals the notebook is moving from toy/synthetic architecture demos to training on an actual labeled dataset.

**Key Concepts**
- Transition from architecture design to applied training

**Q&A**
- Q: What real dataset does the notebook apply its CNN design to next? A: The Flowers102 dataset (102 flower classes).

## Cell 10 (code)
**Loads the Flowers102 dataset with resizing/normalization transforms and builds `DataLoader`s.**
- Defines a `transforms.Compose` pipeline: resize to 32×32, convert to tensor, normalize with mean/std 0.5 across all 3 RGB channels.
- Loads `train_data` and `val_data` via `torchvision.datasets.Flowers102`.
- Wraps both in `DataLoader` objects with `batch_size=32` (train shuffled, val not).
- Library/API: `torchvision.datasets.Flowers102`, `torchvision.transforms`, `torch.utils.data.DataLoader`.

**Key Concepts**
- Data preprocessing pipelines (resize, normalize)
- Train/validation split via dataset `split` parameter
- `DataLoader` batching and shuffling

**Q&A**
- Q: Why resize Flowers102 images to 32×32? A: To match the input size expected by the small CNN architectures designed earlier in the notebook (e.g., CIFAR-style 32×32 input).
- Q: Why is `shuffle=True` used for the training loader but not the validation loader? A: Shuffling training data helps prevent the model from learning order-dependent patterns; validation order doesn't affect evaluation, so shuffling is unnecessary.

## Cell 11 (code)
**Defines `CNN3LayerFlowers`: same 3-conv architecture as `CNN3Layer`, but with 102 output classes.**
- Identical conv/pool structure to `CNN3Layer` (Cell 8).
- Only difference: `fc2 = nn.Linear(128, 102)` to match Flowers102's 102 classes.
- Library/API: `torch.nn`, `torch.nn.functional`.

**Key Concepts**
- Adapting a generic architecture to a specific dataset's class count
- Architecture reuse across tasks

**Q&A**
- Q: What is the only structural difference between `CNN3Layer` and `CNN3LayerFlowers`? A: The final FC layer's output size (10 vs. 102, matching each dataset's class count).

## Cell 12 (code)
**Trains `CNN3LayerFlowers` for 5 epochs using Adam optimizer and cross-entropy loss.**
- Moves the model to GPU if available (`torch.device`).
- Sets up `torch.optim.Adam` (lr=0.001) and `nn.CrossEntropyLoss`.
- Standard training loop: zero gradients, forward pass, compute loss, backward pass, optimizer step, per batch; prints progress per epoch.
- Library/API: `torch.optim.Adam`, `nn.CrossEntropyLoss`, `.to(device)`.

**Key Concepts**
- Standard PyTorch training loop (zero_grad → forward → loss → backward → step)
- Adam optimizer
- Cross-entropy loss for multi-class classification
- Device-agnostic training (`cuda` if available, else `cpu`)

**Q&A**
- Q: What loss function is used, and why is it appropriate here? A: `CrossEntropyLoss`, appropriate for multi-class classification with raw logits and integer class labels.
- Q: How many training epochs are run? A: 5 (noted as a starting point — "Try more epochs later").

## Cell 13 (code)
**Imports evaluation utilities: confusion matrix, classification report, plotting.**
- Imports `confusion_matrix`, `classification_report`, `ConfusionMatrixDisplay` from `sklearn.metrics`, plus `matplotlib.pyplot` and `numpy`.
- Prepares for the model evaluation performed in subsequent cells.

**Key Concepts**
- Model evaluation tooling (sklearn metrics)

**Q&A**
- Q: What three evaluation-related objects are imported from sklearn? A: `confusion_matrix`, `classification_report`, and `ConfusionMatrixDisplay`.

## Cell 14 (code)
**Runs inference on the validation set and collects predictions vs. true labels.**
- Sets `model.eval()` and disables gradient tracking with `torch.no_grad()`.
- Iterates over `val_loader`, computing predicted classes via `torch.max(outputs, 1)`.
- Accumulates predictions and true labels into Python lists (`all_preds`, `all_labels`) for later metric computation.
- Library/API: `torch.no_grad()`, `torch.max`.

**Key Concepts**
- Evaluation mode (`model.eval()`)
- Disabling gradients during inference (`torch.no_grad()`)
- Argmax over class logits to get predicted class

**Q&A**
- Q: Why call `model.eval()` before running validation? A: To disable training-specific behaviors (e.g., dropout, batchnorm updates) so evaluation is deterministic and consistent.
- Q: Why wrap the validation loop in `torch.no_grad()`? A: To avoid tracking gradients during inference, saving memory and compute since no backward pass is needed.

## Cell 15 (code)
**Computes and visualizes the confusion matrix for the Flowers102 validation results.**
- Builds `cm = confusion_matrix(all_labels, all_preds)` and prints its shape (102×102).
- Displays it with `ConfusionMatrixDisplay` on a large figure (20×20) for readability given 102 classes.
- Also attempts to build an `idx_to_class` mapping from the dataset's `class_to_idx`.
- Library/API: `sklearn.metrics.ConfusionMatrixDisplay`, `matplotlib.pyplot`.

**Key Concepts**
- Confusion matrix for multi-class evaluation
- Class index ↔ class name mapping

**Q&A**
- Q: Why is the confusion matrix figure sized so large (20×20)? A: Because Flowers102 has 102 classes, requiring a large plot for the matrix to remain legible.
- Q: What does the confusion matrix reveal that overall accuracy alone would not? A: Which specific classes are commonly confused with each other, not just the aggregate correctness rate.

## Cell 16 (code)
**Prints the full per-class classification report.**
- Calls `classification_report(all_labels, all_preds, digits=3)`, showing precision, recall, F1-score per class (and overall).
- Library/API: `sklearn.metrics.classification_report`.

**Key Concepts**
- Precision, recall, F1-score per class

**Q&A**
- Q: What three key metrics does `classification_report` provide per class? A: Precision, recall, and F1-score.
- Q: Why might per-class metrics be more informative than a single accuracy number for Flowers102? A: With 102 classes and likely class imbalance, some classes may be classified far worse than others, which a single accuracy score would hide.

## Cell 17 (code)
**Empty cell.**
- No content; likely a placeholder for further experimentation.

## Cell 18 (code)
**Empty cell.**
- No content; likely a placeholder for further experimentation.

---

## Notebook-Level Review

**Overall Summary**
This notebook progressively builds CNN architectures of increasing depth — from a single-conv-layer network for grayscale digits, to a two-conv-layer CIFAR-style network, to a three-conv-layer design — carefully tracing tensor shapes at every stage (conv, pool, flatten, FC). It then applies the final 3-layer architecture to a real 102-class dataset (Flowers102), covering the full applied ML workflow: data loading/transforms, training with Adam and cross-entropy loss, and rigorous evaluation using confusion matrices and per-class classification reports.

**Concept Map**
- *Architecture building blocks*: `nn.Conv2d` (channels, kernel size, padding), `nn.MaxPool2d`, `nn.Linear`, ReLU activation, flattening
- *Shape bookkeeping*: no-padding vs. same-padding output size formulas, tracking spatial/channel dimensions through a network
- *Applied training pipeline*: dataset transforms, `DataLoader`, Adam optimizer, `CrossEntropyLoss`, standard train loop
- *Evaluation*: `model.eval()` + `torch.no_grad()`, confusion matrix, classification report (precision/recall/F1)

**Mixed Q&A Quiz**
1. Q: How does the use of `padding=1` in `MyCIFARCNN` (Cell 4) change the shape-tracking compared to `MyFirstCNN` (Cell 2), which has no padding? A: With padding=1, spatial dimensions are preserved after each conv (e.g., 32×32 stays 32×32), so only the pooling layers shrink the size; without padding, every convolution itself shrinks the spatial dimensions.
2. Q: Why does `CNN3LayerFlowers` (Cell 11) reuse the exact same conv/pool structure as `CNN3Layer` (Cell 8) but change only `fc2`? A: The convolutional feature extractor is task-agnostic (same input size, same feature-learning goal); only the final classification layer needs to match the target dataset's number of classes (10 vs. 102).
3. Q: What is the relationship between the design spec in Cell 7 and the implementation in Cell 8? A: Cell 7 specifies the architecture in prose (3 conv blocks with specific channel counts, then 2 FC layers); Cell 8 is the direct PyTorch `nn.Module` implementation of that exact spec.
4. Q: Why is `torch.no_grad()` used during validation (Cell 14) but not during training (Cell 12)? A: Training requires gradients to update weights via backpropagation, while validation only needs a forward pass for evaluation, so disabling gradient tracking saves memory and compute.
5. Q: How do the shape-tracing cells (3, 5, 6) support debugging the actual model code (2, 4)? A: They provide a manual, cell-by-cell derivation of expected tensor shapes at each layer, which can be used to sanity-check that the `nn.Linear` input sizes (e.g., `8*13*13`, `32*8*8`) are correctly computed for the given input dimensions.
