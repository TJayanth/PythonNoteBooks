# Naive Bayes — Class Reference: Notebook Summary

## Cell 1 (markdown)
**Origin of the term "naive" in Naive Bayes.**
- States that "naive" refers to the assumption that all features are independent of each other.
- Independence means presence/absence of one feature doesn't influence another.
- Sets up the conceptual foundation before the formal theorem is introduced.

**Key Concepts**
- Feature independence assumption

**Q&A**
- Q: Why is the algorithm called "naive"? A: Because it naively assumes all input features are conditionally independent, which is rarely true in real data but simplifies computation.

## Cell 2 (markdown)
**Introduces Bayes' Theorem and its four components.**
- Defines Naive Bayes as a probabilistic classifier built on Bayes' Theorem.
- Presents the formula: P(class|data) = P(class) · P(data|class) / P(data).
- Labels each term: posterior, prior, likelihood, and evidence.
- Restates the "naive" assumption as conditional independence of features given the class.

**Key Concepts**
- Bayes' Theorem
- Posterior / Prior / Likelihood / Evidence
- Conditional independence

**Q&A**
- Q: What does the posterior P(class|data) represent? A: The probability that a data point belongs to a class after observing the data.
- Q: What is the "evidence" term in Bayes' Theorem? A: P(data), the total probability of observing the data across all classes, used as a normalizing constant.

## Cell 3 (python — actually prose, not executable)
**Sets up a real-life medical diagnosis example.**
- Describes a rare disease affecting 1 in 1,000 people.
- States the test is 99% accurate: 99% true positive rate, 1% false positive rate.
- This cell is written as plain text inside a code cell, so running it would raise a `SyntaxError`.
- Provides the scenario used by later cells to demonstrate Bayes' Theorem numerically.

**Key Concepts**
- Prior probability
- True positive / false positive rate

**Q&A**
- Q: Would this cell execute without error? A: No — it's descriptive prose typed into a code cell, so Python would raise a syntax error if run as-is.

## Cell 4 (python — actually prose, not executable)
**States the core question to be answered: probability of actually having the disease given a positive test.**
- Frames the problem as computing P(Have | positive).
- Motivates the need for Bayes' Theorem rather than naively trusting the 99% accuracy figure.
- Also not valid executable Python — it's a plain-text question.

**Key Concepts**
- Posterior probability estimation

**Q&A**
- Q: Why isn't the answer simply 99%? A: Because the test's accuracy alone ignores the disease's low prior probability (base rate), which Bayes' Theorem accounts for.

## Cell 5 (raw)
**Table summarizing the known probabilities for the medical example.**
- Lists P(Have)=0.001, P(Haven't)=0.999 as priors.
- Lists P(Positive|Have)=0.99 (true positive rate) and P(Positive|Haven't)=0.01 (false positive rate).
- Serves as the reference table for the manual calculation in the next cell.

**Key Concepts**
- Prior probability
- Conditional probability table

**Q&A**
- Q: What do the two conditional probabilities represent? A: The test's sensitivity (true positive rate) and its false alarm rate (false positive rate).

## Cell 6 (raw)
**Manual step-by-step computation of P(Have | positive) using Bayes' Theorem.**
- Computes P(positive) via the law of total probability: P(Have)·P(Pos|Have) + P(Haven't)·P(Pos|Haven't) = 0.01098.
- Applies Bayes' formula to get P(Have|positive) = (0.001 × 0.99) / 0.01098 ≈ 0.09.
- Demonstrates the counter-intuitive result that a positive test still means only ~9% real chance of disease.

**Key Concepts**
- Law of total probability
- Bayes' Theorem applied numerically

**Q&A**
- Q: What is P(positive) in this example? A: Approximately 0.01098, the combined probability of testing positive whether or not you have the disease.
- Q: Why is the resulting probability so low despite a 99%-accurate test? A: Because the disease is rare (low prior), so false positives from the much larger healthy population outnumber true positives.

## Cell 7 (markdown)
**Highlights the surprising conclusion of the calculation.**
- States plainly that even with a positive test, the probability of actually having the disease is only ~9%.
- Reinforces the lesson about base-rate fallacy / importance of priors.

**Key Concepts**
- Base-rate fallacy

**Q&A**
- Q: What cognitive bias does this result illustrate? A: The base-rate fallacy — ignoring prior probabilities when interpreting conditional evidence.

## Cell 8 (raw)
**Poses three follow-up questions extending the original scenario.**
- Asks for P(Have|negative), P(Haven't|positive), and (repeated) P(Have|negative).
- Sets up the more complete calculation performed in Cell 9.

**Key Concepts**
- Posterior probability for multiple test outcomes

**Q&A**
- Q: Why compute the negative-test posteriors too? A: To fully characterize the test's reliability in both directions, not just for positive results.

## Cell 9 (python)
**Computes all four posterior probabilities (disease/no-disease given positive/negative test) using Python.**
- Defines priors `P_D`, `P_ND` and conditional rates `P_pos_given_D`, `P_neg_given_D`, `P_pos_given_ND`, `P_neg_given_ND`.
- Uses the law of total probability to compute `P_pos` and `P_neg`.
- Applies Bayes' Theorem to compute `P_D_given_pos`, `P_ND_given_pos`, `P_D_given_neg`, `P_ND_given_neg`.
- Prints all four posterior probabilities with formatted precision.
- Libraries/APIs: pure Python arithmetic (no external libraries).

**Key Concepts**
- Bayes' Theorem (programmatic implementation)
- Law of total probability

**Q&A**
- Q: What does `P_D_given_neg` represent, and why is it so small? A: The probability of having the disease given a negative test result; it's tiny because the test has high sensitivity and the disease is rare.
- Q: What would happen if `P_pos_given_D` were lower (a less sensitive test)? A: `P_D_given_pos` would decrease and `P_D_given_neg` would increase, since a less sensitive test misses more true cases.

## Cell 10 (python)
**Visualizes the medical diagnosis scenario as a probability tree using `networkx`.**
- Recomputes the same posterior probabilities as Cell 9.
- Builds a directed graph (`nx.DiGraph`) with nodes for Population → Disease/No Disease → Positive/Negative outcomes.
- Uses fixed `pos` coordinates and `labels` for a manual tree layout.
- Draws edge labels showing the conditional probabilities and annotates the plot with the computed posterior values.
- Libraries/APIs: `matplotlib.pyplot`, `networkx`.

**Key Concepts**
- Probability tree / decision tree visualization
- Directed graphs with `networkx`

**Q&A**
- Q: What does each edge label in the graph represent? A: A conditional probability (e.g., P(Positive|Disease) = 0.99) connecting a parent node to a child outcome.
- Q: Why use a graph instead of just printed numbers? A: A visual tree makes the branching structure of conditional probabilities easier to follow than raw numbers alone.

## Cell 11 (python)
**Empty cell.** No content — likely a spacer between the medical example and the next topic.

## Cell 12 (python)
**Introduces a new example: predicting computer purchases from Age and Income.**
- Comment-only cell stating the classification goal (Buy = Yes/No based on Age and Income).
- Transitions the notebook from the medical example to a categorical classification example.

**Key Concepts**
- Categorical feature classification

**Q&A**
- Q: What are the two features used to predict the target in this example? A: Age and Income.

## Cell 13 (raw)
**Presents the training dataset as a markdown table.**
- Lists 14 rows of Age (Young/Middle/Senior), Income (High/Medium/Low), and Buy (Yes/No).
- This is the dataset the following cells load into a DataFrame and compute statistics on.

**Key Concepts**
- Training dataset for categorical Naive Bayes

**Q&A**
- Q: How many training examples are in this dataset? A: 14 rows.

## Cell 14 (python — actually prose, not executable)
**States the prediction goal: Age=Young, Income=Medium.**
- Frames the target query for the manual Naive Bayes calculation.
- Not valid Python syntax — descriptive text inside a code cell.

**Key Concepts**
- Query instance for classification

**Q&A**
- Q: What specific instance is being classified in this example? A: A person with Age=Young and Income=Medium.

## Cell 15 (python)
**Loads the Buy-a-Computer dataset into a pandas DataFrame.**
- Creates `data` as a `pd.DataFrame` with columns Age, Income, Buy matching the table in Cell 13.
- Calls `data.head()` to preview the first rows.
- Libraries/APIs: `pandas`.

**Key Concepts**
- pandas DataFrame construction

**Q&A**
- Q: What does `data.head()` display? A: The first 5 rows of the DataFrame by default.

## Cell 16 (python)
**Computes prior probabilities P(Yes) and P(No).**
- Filters `data` by `Buy` column and divides by total row count to get class priors.
- Prints both priors formatted to 2 decimal places.
- Libraries/APIs: pandas boolean indexing.

**Key Concepts**
- Prior probability estimation from data

**Q&A**
- Q: How is P(Yes) computed? A: Count of rows where Buy == 'Yes' divided by the total number of rows.

## Cell 17 (python)
**Computes likelihoods P(Age=Young|Yes), P(Income=Medium|Yes), P(Age=Young|No), P(Income=Medium|No).**
- Defines a reusable `likelihood(feature, value, label)` function that filters by class label, then computes the conditional frequency of a feature value.
- Applies it to get four likelihood values needed for the Naive Bayes calculation.
- Libraries/APIs: pandas filtering.

**Key Concepts**
- Likelihood estimation (frequency-based)
- Reusable helper functions

**Q&A**
- Q: What does the `likelihood` function return? A: The proportion of rows with a given feature value within the subset of rows matching a specific class label.
- Q: Why is this function reusable across features? A: It's parameterized by feature name, value, and label, so any feature/value/class combination can be queried.

## Cell 18 (python)
**Computes unnormalized posterior probabilities for Yes and No, and predicts the class.**
- Multiplies prior × likelihood(Age) × likelihood(Income) for each class (Naive Bayes formula assuming feature independence).
- Compares `posterior_yes` vs `posterior_no` to pick the final prediction.
- Prints both posteriors and the final prediction.

**Key Concepts**
- Naive Bayes classification rule
- Unnormalized posterior (proportional scoring)

**Q&A**
- Q: Why can the posteriors be compared without dividing by P(data)? A: Because P(data) is the same constant for both classes, so it doesn't affect which posterior is larger.
- Q: What assumption allows multiplying P(Age=Young|Yes) and P(Income=Medium|Yes) together? A: The naive assumption that Age and Income are conditionally independent given the class.

## Cell 19 (python)
**Normalizes the posterior probabilities to sum to 1 and re-confirms the prediction.**
- Divides each unnormalized posterior by their sum (`total`) to get true probabilities.
- Prints normalized P(Yes|X) and P(No|X).
- Repeats the final prediction logic from Cell 18.

**Key Concepts**
- Probability normalization

**Q&A**
- Q: Why normalize the posteriors here? A: To convert proportional scores into actual probabilities that sum to 1, useful for reporting confidence.

## Cell 20 (markdown)
**Section header: "Types".** Introduces the upcoming overview of Naive Bayes variants.

## Cell 21 (raw)
**Table listing the four scikit-learn Naive Bayes variants and their best-fit data types.**
- GaussianNB → continuous/normal features.
- MultinomialNB → discrete count features (e.g., text word counts).
- BernoulliNB → binary/boolean features.
- CategoricalNB → non-binary categorical features (scikit-learn ≥0.22).

**Key Concepts**
- Naive Bayes variants and their use cases

**Q&A**
- Q: Which variant is best suited for word-count text features? A: `MultinomialNB`.
- Q: Which variant handles binary presence/absence features? A: `BernoulliNB`.

## Cell 22 (python)
**Demonstrates `MultinomialNB` for spam-style text classification using 20 Newsgroups as a proxy.**
- Loads two categories (`sci.space`, `rec.autos`) via `fetch_20newsgroups`.
- Vectorizes text with `CountVectorizer(stop_words='english')` (Bag-of-Words).
- Splits into train/test with `train_test_split`, trains `MultinomialNB`, and evaluates with `classification_report`.
- Libraries/APIs: `sklearn.datasets`, `CountVectorizer`, `MultinomialNB`, `train_test_split`, `classification_report`.

**Key Concepts**
- Bag-of-Words text vectorization
- MultinomialNB for text classification
- Train/test split and classification report

**Q&A**
- Q: Why is `MultinomialNB` chosen instead of `GaussianNB` here? A: Because the features are word counts (discrete), which matches MultinomialNB's assumptions rather than continuous Gaussian-distributed features.
- Q: What would happen if `stop_words='english'` were removed? A: Common words like "the", "is", "and" would be included as features, adding noise and reducing model discrimination.

## Cell 23 (python)
**Demonstrates `GaussianNB` on the Iris dataset (continuous features).**
- Loads `load_iris()` data (`X`, `y`).
- Splits train/test, trains `GaussianNB`, predicts, and prints `accuracy_score`.
- Libraries/APIs: `sklearn.datasets.load_iris`, `GaussianNB`, `accuracy_score`.

**Key Concepts**
- GaussianNB for continuous numeric features
- Accuracy evaluation metric

**Q&A**
- Q: Why is `GaussianNB` appropriate for Iris data? A: Iris features (sepal/petal measurements) are continuous and approximately normally distributed, matching GaussianNB's assumption.

## Cell 24 (python)
**Demonstrates `MultinomialNB` with TF-IDF features on a text classification proxy dataset.**
- Uses `TfidfVectorizer` instead of raw counts to weight words by importance.
- Loads two 20-newsgroups categories (`rec.autos`, `rec.sport.hockey`) as a stand-in for a real movie review sentiment dataset.
- Trains/evaluates `MultinomialNB` with `accuracy_score`.
- Notes in comments that a real sentiment dataset (positive/negative folders) could be loaded via `load_files`.
- Libraries/APIs: `TfidfVectorizer`, `MultinomialNB`, `load_files` (mentioned, unused).

**Key Concepts**
- TF-IDF weighting vs. raw Bag-of-Words counts

**Q&A**
- Q: How does TF-IDF differ from the `CountVectorizer` approach in Cell 22? A: TF-IDF weights word frequency by inverse document frequency, downweighting common words across documents rather than counting them equally.

## Cell 25 (python)
**Demonstrates `CategoricalNB` for a small categorical disease-prediction dataset.**
- Builds a DataFrame with `Fever`, `Cough`, `Disease` columns.
- Encodes categorical string values into integers with `LabelEncoder` for each column.
- Trains/evaluates `CategoricalNB` and prints accuracy.
- Libraries/APIs: `LabelEncoder`, `CategoricalNB`, `accuracy_score`.

**Key Concepts**
- Label encoding categorical features
- CategoricalNB for non-binary categorical data

**Q&A**
- Q: Why must `Fever` and `Cough` be label-encoded before fitting `CategoricalNB`? A: `CategoricalNB` (like most scikit-learn models) requires numeric input, so string categories must be converted to integer codes first.
- Q: With only 8 samples, what's a likely risk of this evaluation? A: High variance / unreliable accuracy estimate due to a very small train/test split.

## Cell 26 (python)
**Demonstrates `BernoulliNB` for binary spam/ham text classification.**
- Uses a small hard-coded list of `texts` and binary `labels` (1=Spam, 0=Ham).
- Vectorizes with `CountVectorizer(binary=True)` to capture word presence/absence rather than counts.
- Trains `BernoulliNB`, evaluates accuracy, then predicts on two new unseen texts.
- Libraries/APIs: `BernoulliNB`, `CountVectorizer(binary=True)`.

**Key Concepts**
- Binary feature vectorization
- BernoulliNB for presence/absence features

**Q&A**
- Q: How does `CountVectorizer(binary=True)` differ from the default? A: It records 1/0 for whether a word appears at all, ignoring how many times it appears, matching BernoulliNB's binary feature assumption.
- Q: Why might accuracy on this tiny dataset be unreliable? A: The dataset has only 6 samples total, split into an even smaller train/test set, so results won't generalize.

## Cell 27 (python)
**Empty cell.** No content — likely a trailing spacer at the end of the notebook.

## Notebook-Level Review

### Overall Summary
This notebook builds Naive Bayes classification from first principles: it starts with Bayes' Theorem and a classic medical-diagnosis example to build intuition about priors, likelihoods, and posteriors (including the base-rate fallacy), then works through a small "Buy a Computer" dataset to manually implement Naive Bayes classification with pandas. It concludes with a survey of the four scikit-learn Naive Bayes variants (`GaussianNB`, `MultinomialNB`, `BernoulliNB`, `CategoricalNB`), demonstrating each on an appropriate toy dataset (medical/categorical data, Iris, 20 Newsgroups text, and small spam examples). Overall, it moves from theory → manual calculation → practical scikit-learn implementations across data types.

### Concept Map
- **Probability theory**: Bayes' Theorem, prior/likelihood/posterior/evidence, law of total probability, base-rate fallacy, probability normalization.
- **Manual Naive Bayes implementation**: frequency-based priors and likelihoods, unnormalized vs. normalized posteriors, independence assumption.
- **Visualization**: probability trees with `networkx`/`matplotlib`.
- **scikit-learn Naive Bayes variants**: `GaussianNB` (continuous), `MultinomialNB` (counts/TF-IDF text), `BernoulliNB` (binary), `CategoricalNB` (categorical).
- **Text feature engineering**: Bag-of-Words (`CountVectorizer`), TF-IDF (`TfidfVectorizer`), binary vectorization.
- **Evaluation**: `accuracy_score`, `classification_report`, train/test splitting.

### Mixed Q&A Quiz
- Q: In the medical example, why did a 99%-accurate test still yield only ~9% true disease probability, and how does this relate to the "prior" in Bayes' Theorem? A: Because the disease's prior probability (1 in 1,000) is very low, the number of false positives from the large healthy population overwhelms the true positives — the posterior is dominated by the prior, not just test accuracy.
- Q: How does the manual likelihood calculation in the Buy-a-Computer example (Cells 16-19) mirror what `MultinomialNB`/`CategoricalNB` do internally? A: Both estimate class priors and per-feature conditional probabilities from training data frequencies, then multiply them assuming feature independence — the manual code is essentially a hand-rolled Categorical/Multinomial Naive Bayes.
- Q: Why would `GaussianNB` be a poor choice for the spam/ham text classification in Cell 26, while `BernoulliNB` works well? A: Text features here are binary word presence/absence, not continuous normally-distributed values, so `GaussianNB`'s Gaussian likelihood assumption doesn't fit; `BernoulliNB` directly models binary feature likelihoods.
- Q: What is the difference in vectorization strategy between Cell 22 (`CountVectorizer`), Cell 24 (`TfidfVectorizer`), and Cell 26 (`CountVectorizer(binary=True)`), and which NB variant pairs with each? A: Cell 22 uses raw word counts paired with `MultinomialNB`; Cell 24 uses TF-IDF-weighted counts also paired with `MultinomialNB`; Cell 26 uses binary presence/absence paired with `BernoulliNB` — each vectorization matches the likelihood model assumed by its NB variant.
- Q: Across the notebook, what is the unifying "naive" assumption that makes all these different NB variants computationally tractable? A: All variants assume features are conditionally independent given the class label, allowing the joint likelihood to be computed as a simple product of per-feature likelihoods rather than requiring a full joint distribution.
