# CNN - addition (weights in Layer).ipynb — Notebook Summary

## Cell 1 (raw)
**Explains that backprop updates each layer's own weights locally by accumulating gradients over all filter positions.**
- States each layer computes gradients of the loss w.r.t. its own weights via the chain rule and updates them independently.
- Clarifies backprop flows backward through layers, but weight updates are local to each layer.
- Key rule for this notebook: gradients are accumulated across all sliding filter positions (spatial), not across filters or layers.
- Sets up the manual walkthrough of computing ∂L/∂W for a single conv filter weight.

**Key Concepts**
- Backpropagation locality (per-layer weight updates)
- Gradient accumulation over sliding window positions
- Chain rule in convolution layers

**Q&A**
- Q: Are gradients accumulated across different filters/layers, or across spatial positions? A: Across spatial positions — each filter weight's gradient sums contributions from every location the filter was applied.
- Q: What does "weight updates are local to each layer" mean? A: Each layer only updates its own weights using its own locally-computed gradient, even though the backward pass flows through the whole network.

## Cell 2 (code — illustrative pseudocode, not runnable)
**Defines a sample 5×5 single-channel input matrix `X` used for the manual gradient walkthrough.**
- Presents `X` as a literal matrix (values 0-2) written as pseudocode, not valid Python syntax as shown.
- Serves as the "toy" input image for hand-computing convolution gradients in the following cells.

**Key Concepts**
- Toy input for manual convolution/gradient example

**Q&A**
- Q: What is the shape of X? A: 5×5 (a single-channel toy image).
- Q: Is this cell executable Python as written? A: No — it's a plain matrix literal without valid assignment syntax; it would raise a SyntaxError if run.

## Cell 3 (code — illustrative pseudocode, not runnable)
**Shows the initial 3×3 filter weights `W` for the conv layer.**
- Presents the starting filter weights (small values around 0 to 0.2) before any gradient update.
- Paired with Cell 2's input to set up the manual forward/backward example.

**Key Concepts**
- Convolution filter (kernel) weights

**Q&A**
- Q: What size is the filter W? A: 3×3.
- Q: What role does this initial W play in the walkthrough? A: It's the starting point that will be updated via `W_new = W - lr·∂L/∂W`.

## Cell 4 (code — illustrative pseudocode, not runnable)
**Presents the assumed upstream gradient `dL_dout`, a 3×3 matrix simulating backprop from the next layer.**
- Represents "how much each output pixel contributed to the loss" — the signal that flows backward into this conv layer.
- This is the δ term used in subsequent manual gradient computation.

**Key Concepts**
- Upstream/downstream gradient (δ) in backprop
- Output feature map gradient

**Q&A**
- Q: What does `dL_dout` represent conceptually? A: The gradient of the loss with respect to each pixel of this layer's output — what backprop "sends" into the layer.
- Q: What shape is `dL_dout` and why? A: 3×3, matching the conv output size for a 5×5 input with a 3×3 filter (no padding, stride 1).

## Cell 5 (code — illustrative pseudocode, not runnable)
**Shows the final computed gradient `grads_W` (3×3) for the filter weights.**
- Presents the result of accumulating gradient contributions from every position the filter was applied.
- States explicitly these values are summed "over all the positions where the filter was applied."

**Key Concepts**
- Accumulated weight gradient (∂L/∂W)

**Q&A**
- Q: How is `grads_W[i,j]` computed conceptually? A: By summing, over every sliding-window position, the product of the upstream gradient at that output location and the corresponding input pixel.

## Cell 6 (code — worked table, not runnable)
**Step-by-step table deriving `W[0,0] = -3.0` by hand from patch positions, input pixels, and δ values.**
- Lists all 9 patch positions with their top-left input pixel, the δ (gradient) at that output location, and their product (contribution).
- Sums the 9 contributions to get `W[0,0] = -3.0`, matching Cell 5's result.
- Makes the accumulation rule from Cell 1 concrete with real numbers.

**Key Concepts**
- Manual gradient accumulation arithmetic
- Correlation between output gradient position and input patch

**Q&A**
- Q: How many terms are summed to get W[0,0]? A: 9 (one per valid 3×3 patch position on the 5×5 input, matching each output location).
- Q: What is each term in the sum? A: δ (gradient at that output position) multiplied by the corresponding input pixel value.

## Cell 7 (code — illustrative pseudocode, not runnable)
**Shows the updated filter weights after one gradient-descent step with learning rate 0.01.**
- Applies the update rule `W_new = W - η·∂L/∂W` to every weight.
- Walks through `W_new[0,0] = 0.1 - (0.01 × -3.0) = 0.13` as a worked example.

**Key Concepts**
- Gradient descent weight update rule
- Learning rate (η)

**Q&A**
- Q: What is the update formula shown? A: W_new = W - η·∂L/∂W.
- Q: Why does W[0,0] increase from 0.1 to 0.13 despite "subtracting" the gradient? A: Because the gradient at that position is negative (-3.0), so subtracting a negative value increases the weight.

## Cell 8 (code)
**NumPy implementation: computes accumulated ∂L/∂W[1,1] contribution across a 3-channel input, step by step, with printed trace.**
- Builds a 3-channel (C,H,W) input `X` and a fixed `dL_dout` (3×3).
- Loops over all 9 valid patch positions, extracting the center pixel of each patch per channel, multiplying by δ, and accumulating into `accumulated`.
- Prints a full step-by-step trace (delta, center pixels, contribution, running total) for pedagogical transparency.
- Library/API: `numpy` (`np.stack`, array slicing, loops).

**Key Concepts**
- Multi-channel convolution gradient accumulation
- Center-weight gradient (∂L/∂W[1,1] specifically)
- Per-channel gradient tracking

**Q&A**
- Q: Why is only the center pixel of each patch used here? A: This cell specifically isolates the gradient computation for the center weight, W[1,1], of the 3×3 filter.
- Q: What does the final printed `accumulated` array represent? A: The per-channel accumulated gradient ∂L/∂W[1,1], summed across all 9 spatial positions.

## Cell 9 (code)
**Generalizes Cell 8: computes the full ∂L/∂W gradient tensor (all 3×3 weights, all channels), not just the center.**
- Initializes `grads_W` with shape (C, 3, 3) and accumulates `delta * patch` for every sliding position.
- Extracts full 3×3 patches per channel via `X[:, i:i+3, j:j+3]` and broadcasts the scalar δ across the patch.
- Prints the final gradient tensor per channel.
- Library/API: `numpy`.

**Key Concepts**
- Full filter-weight gradient tensor
- Vectorized/broadcasted gradient accumulation
- Per-channel independent filters within one conv layer

**Q&A**
- Q: What is the shape of `grads_W` here, and why? A: (C, 3, 3) — one full 3×3 gradient per input channel, since each channel has its own 3×3 kernel weights.
- Q: How does this cell differ from Cell 8? A: Cell 8 computed the gradient for only the center weight (W[1,1]); this cell computes gradients for all 9 weight positions simultaneously.

## Cell 10 (code — explanatory text, not runnable)
**Clarifies that each input channel has its own independent 3×3 kernel within a filter.**
- States a filter applied to a 3-channel input has shape (3,3,3): 3 channels × 3×3 weights each.
- Notes the output pixel is the sum of per-channel convolutions, but weight *gradients* remain per-channel.

**Key Concepts**
- Per-channel kernel weights
- Sum-across-channels forward pass vs. per-channel gradient computation

**Q&A**
- Q: What is the total weight shape for one 3×3 filter applied to a 3-channel input? A: (3, 3, 3) — 3 separate 3×3 kernels, one per channel.
- Q: Are the channel gradients combined into one value? A: No — each channel's gradient is tracked and reported separately, even though the forward output sums contributions across channels.

## Cell 11 (code)
**Visualizes the input channels and their gradients side-by-side using matplotlib.**
- Generates a random 3-channel 5×5 image and computes `grad_W` via the same accumulation loop as Cell 9.
- Plots a 3×3 grid: for each RGB channel, shows the input channel, its ∂L/∂W heatmap, and an example 3×3 patch.
- Library/API: `numpy`, `matplotlib.pyplot`.

**Key Concepts**
- Gradient visualization
- Per-channel heatmaps

**Q&A**
- Q: What three things are plotted per channel? A: The full input channel, the accumulated gradient (∂L/∂W) heatmap, and a sample 3×3 patch.
- Q: What color maps are used to distinguish channels? A: 'Reds', 'Greens', 'Blues' for R, G, B respectively, and 'gray'/'bwr' for gradient magnitude.

## Cell 12 (markdown)
**Prompt question: do you observe separate weights being updated across channels?**
- A reflective question checkpoint asking the learner to confirm the per-channel independence shown in Cells 10-11.

**Key Concepts**
- Self-check / comprehension prompt

**Q&A**
- Q: What is this cell asking the reader to verify? A: Whether the visualization confirms that each channel gets its own independently updated set of weights.

## Cell 13 (code)
**Animates the gradient accumulation process across all 9 patch positions and saves it as a GIF.**
- Repeats the accumulation loop from Cell 11 but captures each step as an animation frame using `matplotlib.animation.ArtistAnimation`.
- Saves the result as `rgb_conv_grad_accum.gif` for visual/temporal understanding of accumulation.
- Library/API: `numpy`, `matplotlib.pyplot`, `matplotlib.animation`.

**Key Concepts**
- Animating gradient accumulation over time/steps
- `ArtistAnimation` for step-by-step visualization

**Q&A**
- Q: What does each animation frame represent? A: One sliding-window step, showing the input, the current patch, and the gradient accumulated so far.
- Q: What file is produced by this cell? A: `rgb_conv_grad_accum.gif`.

## Cell 14 (code)
**Extended animation that also overlays a rectangle showing the current patch location on the input image.**
- Adds a `matplotlib.patches.Rectangle` overlay to visually highlight which 3×3 region of the input is currently contributing to the gradient.
- Saves the animation as `all_patches_grad_accum.gif`.
- Library/API: `numpy`, `matplotlib.pyplot`, `matplotlib.animation`, `matplotlib.patches`.

**Key Concepts**
- Visual localization of the sliding filter window
- Rectangle patch overlays for spatial context

**Q&A**
- Q: What additional visual element does this cell add compared to Cell 13? A: A red rectangle outline on the input image marking the current 3×3 patch position.
- Q: What is the output filename? A: `all_patches_grad_accum.gif`.

## Cell 15 (code — commented-out debug cell)
**Entirely commented-out debugging cell used to isolate an earlier animation bug.**
- All code is commented out; the author's note says "just checking where i made a mistake, the next cell should work fine."
- Was a scratch/debug attempt using a single patch and simple grayscale visualization, saved as `debug_forward.gif` if uncommented.
- Explicitly a debugging artifact — not part of the main narrative flow.

**Key Concepts**
- Debugging animation/artist issues in matplotlib

**Q&A**
- Q: Does this cell execute any code as-is? A: No — every line is commented out, so running it does nothing.
- Q: What was this cell used for? A: Isolating a bug in an earlier animation attempt before the corrected version in the next cell.

## Cell 16 (code)
**Combined forward + backward animation showing activation map alongside gradients.**
- Computes both a dummy forward "activation" (`sum(patch)`) and the backward gradient contribution for each sliding position simultaneously.
- Builds a 4-column animated figure per channel: input (with patch rectangle), extracted patch, accumulated ∂L/∂W, and the evolving activation map.
- Saves as `forward_backward (gradient)_propagation.gif`.
- Library/API: `numpy`, `matplotlib.pyplot`, `matplotlib.animation`, `matplotlib.patches`.

**Key Concepts**
- Joint forward-pass and backward-pass visualization
- Activation map vs. gradient map side-by-side

**Q&A**
- Q: What two neural-network passes does this animation visualize together? A: The forward pass (activation map, via a dummy sum) and the backward pass (gradient accumulation, ∂L/∂W).
- Q: Why use `np.sum(patch)` for activation instead of a real convolution? A: It's a simplified stand-in ("dummy forward") just to illustrate how a per-position activation map builds up alongside the gradient.

## Cell 17 (code)
**Empty cell.**
- No content; likely left as a placeholder or scratch cell.

## Cell 18 (code)
**Empty cell.**
- No content; likely left as a placeholder or scratch cell.

---

## Notebook-Level Review

**Overall Summary**
This notebook manually derives and then programmatically verifies how gradients accumulate for convolutional filter weights during backpropagation. It starts with a fully worked-by-hand example (5×5 input, 3×3 filter, given upstream gradient) showing that each weight's gradient is the sum of δ×input-pixel over every sliding-window position, then generalizes this to multi-channel (RGB) inputs where each channel maintains its own independent kernel and gradient. The second half of the notebook builds increasingly rich visualizations — static plots, then animated GIFs — to make the abstract accumulation process (and eventually the combined forward+backward pass) visually intuitive.

**Concept Map**
- *Backprop fundamentals*: per-layer locality of weight updates, chain rule, gradient descent update rule (W_new = W - η·∂L/∂W)
- *Convolution-specific gradients*: sliding-window accumulation, δ (upstream gradient) × input patch, per-channel independent kernels
- *Manual-to-code bridge*: hand-worked table (Cell 6) validated against NumPy accumulation loops (Cells 8-9)
- *Visualization techniques*: static per-channel heatmaps, `ArtistAnimation` GIFs, patch-highlighting rectangles, combined forward/backward animations

**Mixed Q&A Quiz**
1. Q: In the hand-worked example (Cells 2-7), how does the value W[0,0] = -3.0 relate to the final updated weight 0.13? A: The update rule W_new = W - η·∂L/∂W is applied with η=0.01: 0.1 - (0.01 × -3.0) = 0.13 — a negative gradient increases the weight.
2. Q: Why does a 3×3 filter applied to a 3-channel image have 27 total weights rather than 9? A: Because each channel gets its own independent 3×3 kernel (Cell 10), so total weights = channels × kernel size = 3×3×3=27.
3. Q: What is the conceptual difference between the accumulation shown in Cell 8 versus Cell 9? A: Cell 8 isolates the gradient for a single weight position (the filter center, W[1,1]); Cell 9 computes the full gradient tensor for all 9 weight positions at once.
4. Q: How do the animated GIFs (Cells 13-16) build on the static computations from Cells 8-11? A: They replay the exact same accumulation loop but capture each incremental step as a frame, turning the "sum over positions" concept into a visual, temporal sequence.
5. Q: Why is `np.sum(patch)` used as a stand-in for the forward pass in Cell 16 rather than an actual weighted convolution? A: The animation's purpose is pedagogical — to show forward and backward passes happening in sync — so a simplified activation avoids needing trained weights while still demonstrating the two-pass relationship.
