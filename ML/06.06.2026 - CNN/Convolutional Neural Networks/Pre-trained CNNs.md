# Pre-trained CNNs.ipynb — Notebook Summary

## Cell 1 (markdown)
**Title: "Leveraging Pre-Trained CNNs for Image Tasks."**
- States the notebook's overarching theme: using existing pre-trained CNN architectures rather than training from scratch.

**Key Concepts**
- Transfer learning theme

**Q&A**
- Q: What is this notebook's central theme? A: Leveraging pre-trained CNNs (transfer learning) for image tasks instead of training from scratch.

## Cell 2 (code — explanatory text, not runnable)
**Introduces transfer learning: definition and motivation.**
- Defines transfer learning as using a model trained on one task as a starting point for a related task.
- Lists reasons to use pre-trained CNNs: saves time/compute, helps when labeled data is scarce, often yields better performance than training from scratch.

**Key Concepts**
- Transfer learning
- Data efficiency via pre-training

**Q&A**
- Q: What is transfer learning, per this notebook's definition? A: Using a model trained on one task as the starting point for a related task.
- Q: Name two reasons to prefer pre-trained CNNs over training from scratch. A: They save time/compute, and they help when labeled data is scarce (also often yield better performance).

## Cell 3 (code — explanatory text, not runnable)
**Lists popular pre-trained CNN architectures.**
- Names AlexNet, VGGNet, ResNet, Inception (GoogLeNet), and MobileNet/EfficientNet.
- Gives a one-line distinguishing feature for each (e.g., ResNet's residual connections combat vanishing gradients).

**Key Concepts**
- AlexNet, VGGNet, ResNet, Inception, MobileNet/EfficientNet
- Residual connections, multi-scale extraction, lightweight mobile models

**Q&A**
- Q: What architectural innovation does ResNet introduce? A: Residual (skip) connections, allowing much deeper networks without vanishing gradients.
- Q: Which architectures listed are optimized for mobile/lightweight deployment? A: MobileNet and EfficientNet.

## Cell 4 (code — comparison table, not runnable)
**Table comparing AlexNet, VGG-16, GoogLeNet, ResNet-50, DenseNet-121, MobileNetV2, EfficientNet-B0.**
- Compares year, depth (layers), parameter count, Top-1 ImageNet accuracy, and key feature for each architecture.
- Provides quantitative context for the qualitative descriptions in Cell 3.

**Key Concepts**
- Model depth vs. parameter count vs. accuracy trade-offs
- Top-1 accuracy metric

**Q&A**
- Q: Which architecture in the table has the fewest parameters, and roughly how many? A: GoogLeNet (Inception v1), ~6.8M parameters.
- Q: Which model achieves the highest Top-1 accuracy listed, and what is it? A: EfficientNet-B0, ~77.1%.

## Cell 5 (code — explanatory text, not runnable)
**Explains the Top-1 accuracy / parameter-count trade-off and gives use-case recommendations.**
- Notes lower parameter count is preferred for mobile/edge devices; higher may imply more capacity but more compute.
- Recommends MobileNet/EfficientNet-B0 for mobile/IoT, ResNet-50 for balanced fine-tuning, VGG-16 for simplicity (despite being heavy).

**Key Concepts**
- Accuracy/efficiency trade-offs
- Deployment-driven model selection

**Q&A**
- Q: Which model is recommended for mobile/IoT/embedded projects? A: MobileNet or EfficientNet-B0.
- Q: Which model is recommended as a "balanced trade-off, ideal for fine-tuning"? A: ResNet-50.

## Cell 6 (markdown)
**Section header: Feature Extraction vs Fine-Tuning in Pre-Trained CNNs.**
- Introduces the notebook's second major theme: the two main strategies for reusing pre-trained models.

**Key Concepts**
- Feature extraction vs. fine-tuning

**Q&A**
- Q: What two transfer-learning strategies does this section compare? A: Feature extraction and fine-tuning.

## Cell 7 (code — comparison table, not runnable)
**Table contrasting Feature Extraction and Fine-Tuning across layer freezing, speed, overfitting risk, learning capacity, use case, and typical code.**
- Feature extraction: all pre-trained layers frozen, faster, lower overfitting risk, but limited learning capacity; best with limited/similar data.
- Fine-tuning: top layers unfrozen, slower, higher overfitting risk, greater flexibility; best with more data or a different domain.
- Gives the typical code pattern for each (`base_model.trainable = False/True`, framed in Keras-style but implemented via `requires_grad` in the PyTorch cells that follow).

**Key Concepts**
- Layer freezing (`requires_grad = False`)
- Overfitting risk vs. model capacity trade-off

**Q&A**
- Q: In feature extraction, what happens to the pre-trained layers' weights during training? A: They remain frozen (not updated) — only the new classifier head is trained.
- Q: Why does fine-tuning carry higher overfitting risk than feature extraction? A: More parameters are being updated, giving the model more capacity to memorize a (potentially small) new dataset.

## Cell 8 (code)
**Installs PyTorch via pip.**
- `!pip install torch` — ensures the `torch` package is available before the following demos.

**Key Concepts**
- Environment setup

**Q&A**
- Q: What does this cell do? A: Installs the `torch` package using pip.

## Cell 9 (code)
**Implements feature extraction with a pre-trained ResNet50: freezes all layers, replaces the classifier head.**
- Loads `models.resnet50(pretrained=True)` and freezes every parameter (`requires_grad = False`).
- Replaces `base_model.fc` with a new `nn.Sequential` head: Linear(2048→128) → ReLU → Linear(128→10) → Softmax.
- Moves the model to GPU if available and prints its architecture.
- Library/API: `torch`, `torch.nn`, `torchvision.models`.

**Key Concepts**
- Freezing pre-trained weights (`requires_grad = False`)
- Replacing a model's classification head (`base_model.fc`)
- `nn.Sequential` for building a custom head

**Q&A**
- Q: Why are all base model parameters set to `requires_grad = False`? A: To keep the pre-trained convolutional feature extractor fixed (feature extraction mode) so only the new classifier head is trained.
- Q: What is the new classifier head's output size, and why? A: 10, matching a target 10-class classification problem.

## Cell 10 (code)
**Implements fine-tuning: freezes all layers except `layer4` and `fc`, then replaces the classifier head.**
- Loads a fresh `models.resnet50(pretrained=True)`, freezes all parameters first.
- Selectively unfreezes parameters whose names contain `"layer4"` or `"fc"` (the last residual block and the classifier).
- Replaces `base_model.fc` with the same custom head structure as Cell 9.
- Prints the names of all trainable parameters for verification.
- Library/API: `torch`, `torch.nn`, `torchvision.models`.

**Key Concepts**
- Selective layer unfreezing (partial fine-tuning)
- Fine-tuning only the deepest layers plus the head

**Q&A**
- Q: Which parts of ResNet50 are left trainable in this fine-tuning setup? A: The `layer4` block (the last residual stage) and the `fc` (classifier head) layers.
- Q: Why fine-tune only the last block instead of the whole network? A: The earliest layers learn generic, reusable low-level features (edges, textures), while later layers are more task-specific — fine-tuning only the later layers adapts task-specific features while preserving general ones and reduces overfitting/compute cost.

## Cell 11 (code)
**Empty cell.**
- No content.

## Cell 12 (markdown)
**Section header: Examples.**
- Marks the transition into concrete, runnable demo cells (feature extraction, Grad-CAM, fine-tuning training loop).

**Key Concepts**
- Section transition marker

**Q&A**
- Q: What does this section contain relative to the earlier conceptual cells? A: Hands-on runnable examples building on the feature-extraction/fine-tuning concepts already introduced.

## Cell 13 (code)
**Installs `torch` and `torchvision`.**
- `!pip install torch torchvision`.

**Key Concepts**
- Environment setup

**Q&A**
- Q: What packages does this cell install? A: `torch` and `torchvision`.

## Cell 14 (code)
**Extracts a 512-dim feature vector from an image using pre-trained ResNet18 (feature extraction, layer removed).**
- Loads `models.resnet18(pretrained=True)` in eval mode.
- Builds `feature_extractor` by removing the final FC layer (`nn.Sequential(*list(model.children())[:-1])`).
- Preprocesses a redpanda image (resize 224×224, tensor, ImageNet normalization), runs it through the extractor with `torch.no_grad()`.
- Flattens output to a [1, 512] feature vector and displays the image with the feature-shape as the title.
- Library/API: `torch`, `torchvision.models`, `torchvision.transforms`, `PIL.Image`, `matplotlib.pyplot`.
- Note: the notebook's hardcoded image path uses a Windows path without a raw string (`"C:\Users\...`), which would trigger invalid-escape-sequence warnings/errors in Python — flagged as a potential issue rather than guessed as intentional.

**Key Concepts**
- Removing the classifier head to get raw feature embeddings
- `nn.Sequential(*list(model.children())[:-1])` pattern
- `torch.no_grad()` for inference-only feature extraction

**Q&A**
- Q: What is the shape of the extracted feature vector, and why? A: [1, 512] — ResNet18's penultimate layer (before the FC head) outputs 512 features per image.
- Q: How is the FC layer removed from the pre-trained model? A: By taking `list(model.children())[:-1]` (all layers except the last) and wrapping them in a new `nn.Sequential`.

## Cell 15 (code)
**Plots the 512-dimensional extracted feature vector as a line chart.**
- Converts the feature tensor to a NumPy array (`features.squeeze().numpy()`).
- Plots feature value vs. feature index using `matplotlib.pyplot.plot`.
- Library/API: `numpy`, `matplotlib.pyplot`.

**Key Concepts**
- Visualizing high-dimensional embeddings as a 1D signal

**Q&A**
- Q: What does the x-axis represent in this plot? A: The feature index (0 to 511) of the ResNet18 embedding vector.

## Cell 16 (code)
**Visualizes the same feature vector as a 1×512 heatmap using seaborn.**
- Reshapes the feature vector to `[1, 512]` and renders it with `sns.heatmap`.
- Library/API: `seaborn`, `matplotlib.pyplot`.

**Key Concepts**
- Heatmap visualization of embeddings

**Q&A**
- Q: Why is `feature_np[np.newaxis, :]` used before plotting? A: To reshape the 1D 512-element vector into a 2D (1, 512) array, which `sns.heatmap` requires.

## Cell 17 (code — comment only)
**Comment explaining what Grad-CAM does.**
- States Grad-CAM highlights image regions most important for a specific class prediction.
- Sets up the purpose of the following implementation cells.

**Key Concepts**
- Grad-CAM (Gradient-weighted Class Activation Mapping)

**Q&A**
- Q: What does Grad-CAM visualize? A: The regions of an input image that most influenced the model's prediction for a specific class.

## Cell 18 (code)
**Implements Grad-CAM on ResNet18's `layer4[1].conv2`, producing a heatmap overlay for the predicted class.**
- Registers forward and backward hooks on `model.layer4[1].conv2` to capture activations and gradients.
- Runs a forward pass, takes the predicted class, backpropagates from that class's logit.
- Computes Grad-CAM: averages gradients per channel as weights, weights the activations, sums, applies ReLU, normalizes, and resizes to 224×224.
- Overlays the CAM heatmap (via OpenCV's `COLORMAP_JET`) onto the original image and plots original/heatmap/overlay side by side.
- Library/API: `torch`, `torchvision.models`, `torchvision.transforms`, `cv2`, `numpy`, `matplotlib.pyplot`.

**Key Concepts**
- Grad-CAM algorithm: hooks, gradient-weighted activation maps, ReLU + normalization
- Forward/backward hooks (`register_forward_hook`, `register_backward_hook`)
- Heatmap overlay via OpenCV colormaps

**Q&A**
- Q: What two things do the hooks capture in this Grad-CAM implementation? A: The forward hook captures the layer's activations; the backward hook captures the gradients flowing into that layer.
- Q: Why is ReLU applied to the CAM before normalizing? A: To keep only the positive influence on the predicted class, discarding negative contributions (regions that argue against the class).

## Cell 19 (code — comment only)
**Comment marking a new sub-section: "across layers."**
- Signals the next cell will compare Grad-CAM outputs across multiple network depths.

**Key Concepts**
- Cross-layer Grad-CAM comparison

**Q&A**
- Q: What will the following cell compare that this cell (Cell 18) did not? A: Grad-CAM heatmaps computed at multiple different layers (shallow vs. deep) rather than just one.

## Cell 20 (code)
**Runs Grad-CAM at three different depths (conv1, layer1[0].conv2, layer4[1].conv2) and compares heatmaps.**
- Loops over a dict of target layers, registering/removing hooks for each in turn.
- For each layer: forward pass, backward pass from the predicted class, computes the same Grad-CAM formula as Cell 18.
- Plots a grid comparing original image, heatmap, and overlay for each of the three layers.
- Library/API: `torch`, `torchvision.models`, `cv2`, `numpy`, `matplotlib.pyplot`.

**Key Concepts**
- Layer-depth comparison of learned representations
- Shallow layers capture low-level features (edges/textures); deep layers capture high-level, class-specific features

**Q&A**
- Q: What is expected to differ between the Grad-CAM heatmap at `conv1` versus `layer4[1].conv2`? A: `conv1`'s heatmap tends to reflect low-level, diffuse edge/texture patterns, while `layer4[1].conv2`'s heatmap should more sharply localize the semantically relevant object region driving the class prediction.
- Q: Why must hooks be removed (`h1.remove()`, `h2.remove()`) after each layer's pass? A: To prevent hooks from accumulating and firing on subsequent forward/backward passes for different layers, which would produce incorrect or duplicated captures.

## Cell 21 (code)
**Visualizes raw feature maps (not Grad-CAM) from conv1, layer1[0].conv2, and layer4[1].conv2 as animated GIFs.**
- Registers forward hooks (via a hook-factory `hook_fn`) on the three target layers to capture their raw output feature maps.
- Runs one forward pass, then removes hooks.
- For each layer, animates the first 16 filter activation maps as a grid of frames, saving each as a `.gif` (e.g., `conv1_filters.gif`).
- Library/API: `torch`, `torchvision.models`, `torchvision.transforms`, `matplotlib.pyplot`, `matplotlib.animation`, `numpy`.

**Key Concepts**
- Feature map visualization (raw activations, not gradient-weighted)
- Hook factories for named-layer capture
- Visualizing what different filters "see"

**Q&A**
- Q: How many filters' activations are visualized per layer? A: The first 16 filters (`fmap[:16]`).
- Q: How does this cell's visualization differ conceptually from the Grad-CAM cells (18, 20)? A: This shows raw forward-pass activations for arbitrary filters, without any relation to a specific predicted class or gradient weighting; Grad-CAM specifically highlights class-relevant regions using gradients.

## Cell 22 (code)
**Imports for a fine-tuning training pipeline (datasets, models, optimizer, DataLoader).**
- Imports `torch`, `torch.nn`, `torch.optim`, `torchvision.datasets`/`models`/`transforms`, `DataLoader`, `time`.
- Prepares the imports needed for the full fine-tuning training loop that follows.

**Key Concepts**
- Setup for a supervised fine-tuning pipeline

**Q&A**
- Q: What is this cell preparing for? A: A full model fine-tuning training/validation loop on a custom image dataset.

## Cell 23 (markdown)
**Section header: Fine tuning.**
- Marks the beginning of the complete fine-tuning workflow implementation.

**Key Concepts**
- Fine-tuning section marker

**Q&A**
- Q: What does this heading introduce? A: A full end-to-end fine-tuning example (data loading, model setup, training loop) using ResNet18.

## Cell 24 (code)
**Full fine-tuning pipeline: loads a custom `ImageFolder` dataset, adapts ResNet18's head, and trains/validates for 3 epochs.**
- Defines train/val transforms (resize 224×224, random horizontal flip for train, ImageNet normalization).
- Loads `datasets.ImageFolder` for `finetune/train` and `finetune/val`, wraps in `DataLoader`s (batch_size=32).
- Loads `models.resnet18(pretrained=True)`, replaces `model.fc` with `nn.Linear(in_features, num_classes)` sized to the dataset's actual classes.
- Uses `nn.CrossEntropyLoss` and `optim.Adam(lr=1e-4)`.
- Runs a full train+val loop per epoch, tracking running loss/accuracy, using `torch.set_grad_enabled(phase=='train')` to toggle gradient tracking per phase.
- Library/API: `torch`, `torch.nn`, `torch.optim`, `torchvision.datasets.ImageFolder`, `torchvision.models`, `torchvision.transforms`, `DataLoader`.

**Key Concepts**
- `ImageFolder` for custom labeled image datasets
- Adapting `model.fc` to a new number of classes
- `torch.set_grad_enabled()` for train/val phase switching
- Running loss/accuracy tracking across epochs

**Q&A**
- Q: How does the code determine the correct output size for the replaced `model.fc`? A: `num_classes = len(image_datasets['train'].classes)`, inferred automatically from the `ImageFolder` dataset's subdirectory structure.
- Q: What does `torch.set_grad_enabled(phase == 'train')` accomplish? A: It enables gradient tracking during the training phase and disables it during validation, within a single shared code path for both phases.
- Q: Why is `RandomHorizontalFlip` applied only to the training transform, not validation? A: It's a data augmentation technique meant to increase training data diversity; validation should evaluate on unmodified, real images for a consistent, unbiased metric.

## Cell 25 (code — empty/placeholder)
**Empty cell.**
- No content.

## Cell 26 (code — commented-out scratch code)
**Commented-out imports, apparently a leftover/duplicate scratch cell.**
- Contains only commented-out import statements (`torch`, `torch.nn`, `torch.optim`, `DataLoader`), likely an abandoned draft.
- No functional contribution as-is.

**Q&A**
- Q: Does this cell execute anything? A: No — every line is commented out.

---

## Notebook-Level Review

**Overall Summary**
This notebook is a comprehensive tour of transfer learning with pre-trained CNNs: it starts conceptually (what transfer learning is, why it's used, a comparison of popular architectures like AlexNet/VGG/ResNet/MobileNet/EfficientNet), then contrasts the two core reuse strategies — feature extraction (freeze everything, replace the head) and fine-tuning (selectively unfreeze deeper layers). It follows with hands-on PyTorch/ResNet examples: extracting and visualizing 512-dim feature embeddings, implementing Grad-CAM to interpret which image regions drive predictions (at a single layer and across multiple depths), visualizing raw filter activations as animated GIFs, and finally a complete end-to-end fine-tuning pipeline on a custom `ImageFolder` dataset with a full train/validation loop.

**Concept Map**
- *Transfer learning theory*: definition, motivation, popular architectures (AlexNet, VGG, ResNet, Inception, MobileNet/EfficientNet), accuracy/parameter trade-offs
- *Reuse strategies*: feature extraction (`requires_grad=False`) vs. fine-tuning (selective unfreezing), typical code patterns
- *Feature embeddings*: extracting penultimate-layer features, visualizing as line plots/heatmaps
- *Model interpretability*: Grad-CAM (hooks, gradient-weighted activation maps), raw feature-map visualization, comparing shallow vs. deep layer representations
- *Applied fine-tuning*: `ImageFolder`, adapting the classifier head, full train/val loop with `torch.set_grad_enabled`

**Mixed Q&A Quiz**
1. Q: How does the feature-extraction code in Cell 9 relate to the feature-extraction demo in Cell 14? A: Cell 9 freezes ResNet50 and replaces its head for a *new classification task*; Cell 14 removes ResNet18's head entirely to obtain raw *embedding vectors* for downstream use (e.g., similarity, visualization) rather than classification.
2. Q: Why does the fine-tuning setup in Cell 10 unfreeze only `layer4` and `fc`, while the full fine-tuning pipeline in Cell 24 doesn't freeze any layers at all? A: Cell 10 demonstrates selective/partial fine-tuning (a middle ground between full freezing and full fine-tuning); Cell 24's pipeline updates the whole pre-trained network end-to-end for the new dataset, representing full fine-tuning.
3. Q: How does Grad-CAM (Cells 18/20) differ from the raw feature-map visualization (Cell 21) in terms of what gradients are used? A: Grad-CAM explicitly backpropagates from a specific predicted class's logit and weights activations by their gradients to highlight class-relevant regions; the raw feature-map visualization only uses forward-pass activations with no gradient computation at all.
4. Q: Based on the architecture comparison table (Cell 4) and use-case guidance (Cell 5), which model would best fit the fine-tuning pipeline in Cell 24 if deployment were on a mobile device instead of a server? A: MobileNetV2 or EfficientNet-B0, since they're explicitly recommended for mobile/IoT/embedded use due to their lower parameter counts.
5. Q: What is the common purpose across the Grad-CAM (Cells 18, 20) and feature-map animation (Cell 21) cells, despite their different techniques? A: Both aim to make the pre-trained CNN's internal representations interpretable/visual — Grad-CAM shows *where* the network looks for a specific prediction, while the filter animations show *what patterns* individual filters at different depths respond to.
