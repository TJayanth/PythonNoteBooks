# Simple Project NB: Notebook Summary

## Cell 1 (python)
**Title comment for the project.**
- States the project name: "Support Ticket Auto-Router (NB Classifier)".
- Purely a comment, no executable logic.

**Key Concepts**
- Project framing / problem statement

**Q&A**
- Q: What is the overall goal of this notebook? A: To build a Naive Bayes classifier that automatically routes support tickets to the correct department.

## Cell 2 (python — actually prose, not executable)
**Describes the classification problem in plain language.**
- Gives example ticket texts (WiFi issue, salary issue, email access issue).
- States the three target categories: IT Support, HR, Finance.
- Written as unquoted prose inside a code cell, so it would raise a `SyntaxError` if executed as-is.

**Key Concepts**
- Multi-class text classification problem definition

**Q&A**
- Q: What are the three possible categories a ticket can be routed to? A: IT Support, HR, and Finance.
- Q: Would this cell run without error? A: No — it's plain descriptive text, not valid Python syntax.

## Cell 3 (python — actually prose, not executable)
**Outlines the system design/architecture for the ticket router.**
- Describes four stages: (1) text input, (2) preprocessing via cleaning + TF-IDF vectorization, (3) modeling with scikit-learn `MultinomialNB`, (4) output of category prediction plus confidence score.
- Also written as prose, not valid Python.

**Key Concepts**
- ML system design (input → processing → model → output)
- TF-IDF vectorization
- Confidence scoring

**Q&A**
- Q: What model is planned for the classification stage? A: scikit-learn's `MultinomialNB` (Multinomial Naive Bayes).
- Q: What two outputs does the system aim to produce? A: A predicted category and a confidence score.

## Cell 4 (python)
**Defines a small hard-coded sample dataset of tickets and labels.**
- Creates a Python dict `data` with `"text"` (8 example ticket strings) and `"label"` (corresponding `IT`, `HR`, `Finance` categories).
- Used as a lightweight stand-in dataset before scaling to the full CSV.

**Key Concepts**
- Small labeled dataset for prototyping

**Q&A**
- Q: How many example tickets are in this sample dataset? A: 8.
- Q: Why start with a tiny hard-coded dataset instead of the full CSV immediately? A: To quickly prototype and validate the pipeline logic before scaling up to real, larger data (`support_tickets_10k.csv`).

## Cell 5 (python)
**Commented-out code for loading the full ticket dataset from CSV.**
- Shows (but does not execute) `pd.read_csv("support_tickets_10k.csv")` plus `df.head()` and `df.shape` calls, all commented out.
- Indicates an alternative/future data source once the small hard-coded example is validated.

**Key Concepts**
- CSV data loading with pandas (planned but disabled)

**Q&A**
- Q: Why are these lines commented out rather than deleted? A: To keep the option visible for later use — swapping to the larger `support_tickets_10k.csv` dataset — without cluttering current execution.

## Cell 6 (python)
**Trains the actual Naive Bayes ticket classifier pipeline.**
- Converts the sample `data` dict into a pandas DataFrame `df`.
- Splits `df["text"]`/`df["label"]` into train/test sets (75/25, `random_state=42`).
- Builds an sklearn `Pipeline` of `TfidfVectorizer(stop_words='english')` + `MultinomialNB`.
- Fits the pipeline and prints a `classification_report` on the test predictions.
- Libraries/APIs: `pandas`, `train_test_split`, `TfidfVectorizer`, `MultinomialNB`, `Pipeline`, `classification_report`.

**Key Concepts**
- TF-IDF + MultinomialNB text classification pipeline
- Train/test split evaluation

**Q&A**
- Q: What preprocessing step does `TfidfVectorizer(stop_words='english')` perform? A: It converts raw ticket text into TF-IDF weighted numeric vectors while filtering out common English stop words.
- Q: With only 8 samples split 75/25, what's a key limitation of this evaluation? A: The test set has only 2 samples, so the `classification_report` metrics are statistically unreliable and mainly illustrate the workflow rather than true model performance.

## Cell 7 (python)
**Defines a reusable prediction function with confidence scoring.**
- `predict_ticket(text)` calls `model.predict([text])` for the category and `model.predict_proba([text])` for class probabilities.
- Computes `confidence` as the maximum predicted probability, rounded to 2 decimals and expressed as a percentage.
- Returns a tuple `(prediction, confidence)`.
- Libraries/APIs: uses the `model` pipeline's `.predict()` and `.predict_proba()`.

**Key Concepts**
- Confidence scoring via `predict_proba`
- Wrapping model inference in a reusable function

**Q&A**
- Q: How is the confidence score calculated? A: As the maximum class probability from `model.predict_proba([text])[0]`, multiplied by 100 and rounded to 2 decimal places.
- Q: Why does `predict_ticket` wrap `text` in a list (`[text]`)? A: Because scikit-learn's `predict`/`predict_proba` expect an iterable of samples, even for a single input.

## Cell 8 (python)
**Tests the prediction function on two new example tickets.**
- Calls `predict_ticket("My salary is not credited")` and `predict_ticket("Laptop is overheating and slow")`.
- Prints the resulting (prediction, confidence) tuples to sanity-check the trained model on unseen text.

**Key Concepts**
- Manual inference testing / sanity checks

**Q&A**
- Q: What category would you expect for "My salary is not credited"? A: Finance, based on the similar "Salary not credited this month" example in the training data.

## Cell 9 (python)
**Empty cell.** No content — likely a trailing placeholder at the end of the notebook.

## Notebook-Level Review

### Overall Summary
This notebook implements a small end-to-end "Support Ticket Auto-Router" project, applying Naive Bayes text classification to route support tickets into IT, HR, or Finance categories. It progresses from problem framing and system design, through a tiny hard-coded dataset, to training a TF-IDF + `MultinomialNB` pipeline, and finally wraps inference in a `predict_ticket` helper that returns both a predicted category and a confidence score. It mirrors the structure of the IMDB sentiment project in the Extension notebook but applies it to a multi-class (rather than binary) ticket-routing problem, and hints at scaling to a larger CSV dataset (`support_tickets_10k.csv`).

### Concept Map
- **Problem framing**: multi-class text classification, system design (input → processing → model → output).
- **Data handling**: small hard-coded dict dataset vs. commented-out CSV loading for scaling up.
- **Modeling**: TF-IDF vectorization, `MultinomialNB`, scikit-learn `Pipeline`.
- **Evaluation**: `classification_report`, train/test split.
- **Inference/confidence scoring**: `predict_proba`, wrapping predictions in a reusable function.

### Mixed Q&A Quiz
- Q: How do the system design steps in Cell 3 map onto the concrete code in Cells 6 and 7? A: "Processing" (cleaning + TF-IDF) maps to `TfidfVectorizer` in the pipeline, "Model" (MultinomialNB) maps to the `nb`/`MultinomialNB` step, and "Output" (category + confidence) maps to `predict_ticket`'s use of `predict` and `predict_proba`.
- Q: Why is `MultinomialNB` a reasonable choice for both this ticket-routing project and the IMDB sentiment project in the Extension notebook? A: Both problems use TF-IDF text features (word-frequency-like data), which matches MultinomialNB's assumption of multinomially-distributed count/frequency features.
- Q: If the notebook switched from the hard-coded 8-row dataset to the full `support_tickets_10k.csv` (Cell 5), what would likely improve, and why? A: The `classification_report` metrics and `predict_ticket` confidence scores would become far more statistically meaningful, since a larger, more diverse training set better represents real vocabulary and category boundaries.
- Q: What is the key difference between this notebook's multi-class classification and the binary classification in the IMDB Extension notebook? A: This notebook predicts among 3 classes (IT, HR, Finance) using `MultinomialNB`'s native multi-class support, while the IMDB notebook predicts between 2 sentiment classes (positive/negative).
- Q: Both this notebook and the Class Reference notebook manually or semi-manually compute posterior confidence. How does `predict_proba` in Cell 7 relate to the manual posterior normalization done in the Class Reference notebook (Cell 19 there)? A: Both ultimately normalize class scores into probabilities that sum to 1; `predict_proba` does internally in scikit-learn what the Class Reference notebook did by hand with `posterior_yes / total` and `posterior_no / total`.
