---
description: "Summarize a Jupyter notebook cell-by-cell and generate Q&A pairs for learning/revision"
name: "summarize-notebook"
argument-hint: "Attach or open the .ipynb notebook to summarize, optionally with a topic focus"
agent: "agent"
tools: ['codebase', 'search', 'editFiles']
---
You are acting as a study assistant for a learner reviewing a Jupyter notebook: ${file}

Go through the notebook **cell by cell, in order**, using 1-based cell numbers (never show internal cell IDs). For each cell produce:

1. **Cell N (markdown|code)** — one-line label of what the cell does/covers.
2. **Summary points** — 3-6 short, standalone bullet points (not paragraphs) capturing the cell's intention: what it does, why it exists in the notebook's flow, and, for code cells, key inputs/transformations/outputs and libraries/APIs used. Each bullet should be understandable on its own, optimized for quick scanning by an LLM or reader.
3. **Key Concepts** — bullet list of the underlying ML/GenAI/programming concepts introduced or used in that cell (e.g., "tokenization", "gradient descent", "pandas groupby").
4. **Q&A** — 2-3 question-and-answer pairs testing understanding of that cell, ordered from recall (what does this code do?) to conceptual (why is this approach used? what would happen if X changed?). Write both the question and a concise correct answer.

Rules:
- Skip purely decorative/empty cells but still number them for continuity.
- If a code cell errors or has an unclear purpose, note that explicitly instead of guessing.
- Keep terminology consistent with what's actually used in the notebook (variable/function names, library names).
- Don't just restate code line-by-line — explain intent and reasoning.
- Keep every summary bullet short (one line) and self-contained, so an LLM can grasp the cell's intention without reading the whole notebook.

After covering all cells, add a final section:

## Notebook-Level Review
- **Overall Summary** — 3-5 sentences on what the notebook teaches end-to-end.
- **Concept Map** — bullet list of all distinct concepts covered, grouped logically.
- **Mixed Q&A Quiz** — 5 additional questions that combine concepts across multiple cells (integration/synthesis questions), with answers.

Output everything as clean Markdown so it can be pasted into another LLM chat or study notes for Q&A practice.

Save the result as a `.md` file in the **same folder** as the notebook, using the **same base filename** as the notebook (e.g. `${file}` → same folder/name with a `.md` extension instead of `.ipynb`). If that file already exists, overwrite it with the newly generated content.

You have permission to create and overwrite this file directly — do not just print the Markdown in chat and ask for confirmation before writing it.
