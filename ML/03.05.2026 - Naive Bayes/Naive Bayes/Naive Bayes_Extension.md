# Naive Bayes Extension: Notebook Summary

## Cell 1 (python)
**Trains a full IMDB sentiment classification pipeline with TF-IDF + MultinomialNB.**
- Loads `IMDB.csv` into a pandas DataFrame with `review` and `sentiment` columns.
- Cleans review text via `clean_text`: strips `<br />` HTML tags, removes non-alphabetic characters, lowercases.
- Splits data into train/test with `train_test_split` (80/20, `random_state=42`).
- Builds an sklearn `Pipeline` combining `TfidfVectorizer(max_features=20000, stop_words='english')` and `MultinomialNB`.
- Fits the pipeline, predicts on test data, and prints `accuracy_score` and `classification_report`.
- Libraries/APIs: `pandas`, `re`, `sklearn.model_selection.train_test_split`, `TfidfVectorizer`, `MultinomialNB`, `Pipeline`, `accuracy_score`, `classification_report`.

**Key Concepts**
- Text preprocessing/cleaning with regex
- TF-IDF vectorization
- scikit-learn `Pipeline` composition
- MultinomialNB for binary sentiment classification
- Train/test evaluation metrics

**Q&A**
- Q: What does `clean_text` remove from each review? A: HTML `<br />` tags and any non-alphabetic characters, and it lowercases the text.
- Q: Why use a `Pipeline` instead of calling `TfidfVectorizer` and `MultinomialNB` separately? A: A pipeline bundles preprocessing and modeling into one object, ensuring the same fitted vectorizer is reused consistently for both training and prediction (and simplifies later steps like saving the whole pipeline).
- Q: What would `max_features=20000` control, and what happens if it were lowered? A: It caps the vocabulary to the 20,000 most frequent terms; lowering it would shrink the vocabulary further, potentially losing informative but less frequent words and reducing model expressiveness.

## Cell 2 (python)
**Persists the trained pipeline to disk.**
- Uses `joblib.dump(model, "sentiment_model.pkl")` to serialize the fitted `Pipeline` (vectorizer + classifier together).
- Enables the model to be reloaded later (e.g., for a Flask app) without retraining.
- Libraries/APIs: `joblib`.

**Key Concepts**
- Model serialization/persistence

**Q&A**
- Q: Why save the entire `Pipeline` rather than just the `MultinomialNB` model? A: The pipeline includes the fitted `TfidfVectorizer`, so saving it preserves the exact vocabulary/IDF weights needed to transform new raw text consistently at inference time.

## Cell 3 (python)
**Installs plotting/analysis libraries.**
- Contains `pip install matplotlib wordcloud seaborn` without a `!` or `%` magic prefix, so it would raise a `SyntaxError` if run as Python code in a notebook (should be `!pip install ...`).
- Intent is to ensure dependencies for the following visualization cells are available.

**Key Concepts**
- Package installation in notebooks (shell vs. Python cells)

**Q&A**
- Q: Would this cell run successfully as-is? A: No — Jupyter requires a `!` (or `%pip`) prefix to run shell/pip commands from a code cell; without it, Python attempts to parse `pip install matplotlib wordcloud seaborn` as an expression and errors.

## Cell 4 (python)
**Plots sentiment class distribution as a pie chart.**
- Calls `df["sentiment"].value_counts().plot.pie(autopct="%1.1f%%")` to visualize the proportion of positive vs. negative reviews.
- Adds a title and displays the plot with `plt.show()`.
- Libraries/APIs: `matplotlib.pyplot`, pandas `.value_counts()` and `.plot.pie()`.

**Key Concepts**
- Class distribution visualization
- pandas built-in plotting

**Q&A**
- Q: What does this pie chart help verify before modeling? A: Whether the dataset is balanced between positive and negative classes, which affects how accuracy should be interpreted.

## Cell 5 (python)
**Generates word clouds for positive vs. negative reviews.**
- Concatenates all positive reviews into `positive_text` and negative reviews into `negative_text` using pandas filtering and `" ".join(...)`.
- Builds a `WordCloud(width=800, height=400)` from the positive text and displays it with `plt.imshow`.
- Note: only the positive word cloud is generated/shown; the negative one is computed but not plotted in this cell.
- Libraries/APIs: `wordcloud.WordCloud`, `matplotlib.pyplot`.

**Key Concepts**
- Word cloud visualization for text data exploration

**Q&A**
- Q: What does the size of a word in the word cloud represent? A: Its frequency — larger words appear more often in the positive reviews text.
- Q: Is `negative_text` used anywhere in this cell? A: It's computed but never plotted — only `positive_text` is turned into a word cloud here.

## Cell 6 (python)
**Installs Flask for a future web app.**
- Runs `!pip install flask`, correctly prefixed with `!` to execute as a shell command.
- Suggests the notebook's model is intended to be served via a Flask app (consistent with the `app.py`/`templates/index.html` files in the project folder).
- Libraries/APIs: shell/pip.

**Key Concepts**
- Deploying an ML model behind a web framework

**Q&A**
- Q: Why install Flask in this notebook? A: To prepare for building a web interface/API (seen elsewhere in the project as `app.py`) that serves predictions from the saved `sentiment_model.pkl`.

## Cells 7–10 (python)
**Empty cells.** No content — likely placeholders left at the end of the notebook for further experimentation.

## Notebook-Level Review

### Overall Summary
This notebook extends the earlier Naive Bayes reference material into an applied, end-to-end sentiment analysis project on the IMDB dataset. It cleans raw review text, trains a TF-IDF + `MultinomialNB` pipeline, evaluates it, and serializes the trained pipeline with `joblib` for reuse. It then explores the data visually (sentiment distribution pie chart, word clouds) and prepares for deployment by installing Flask, connecting the modeling work to the `app.py` web app found in the same project folder.

### Concept Map
- **Text preprocessing**: regex-based cleaning, lowercasing, HTML tag removal.
- **Modeling**: TF-IDF vectorization, `MultinomialNB`, scikit-learn `Pipeline`.
- **Evaluation**: `accuracy_score`, `classification_report`.
- **Persistence**: `joblib.dump` for saving trained pipelines.
- **Exploratory visualization**: pie charts (`pandas.plot.pie`), word clouds (`WordCloud`).
- **Deployment prep**: installing `flask` for serving the model via a web app.

### Mixed Q&A Quiz
- Q: How does saving the model with `joblib` (Cell 2) connect to the Flask installation (Cell 6)? A: The saved `sentiment_model.pkl` pipeline can be loaded inside a Flask app to serve real-time sentiment predictions without retraining, linking the modeling and deployment stages of the project.
- Q: If `clean_text` were skipped entirely, how might the pie chart in Cell 4 and word clouds in Cell 5 be affected? A: The pie chart (based on the `sentiment` label counts) would be unaffected, but word clouds would include noisy tokens (HTML artifacts, punctuation, mixed case) since they depend on cleaned review text.
- Q: Why might `MultinomialNB` with TF-IDF be a reasonable baseline for IMDB sentiment analysis, despite the "naive" independence assumption being unrealistic for language? A: Word-presence patterns (e.g., "terrible", "amazing") are often strongly and independently predictive of sentiment in practice, so even with unrealistic independence assumptions, NB provides a fast, interpretable baseline.
- Q: What is the practical risk of installing packages via a plain `pip install ...` line (Cell 3) instead of `!pip install ...`? A: It fails immediately with a syntax error since Jupyter needs the `!` (or `%pip`) prefix to route the line to the shell instead of the Python interpreter.
- Q: How would you extend this notebook to also visualize the negative-review word cloud that was computed in Cell 5 but never displayed? A: Call `WordCloud(...).generate(negative_text)` and pass the result to a second `plt.imshow(...)` call (ideally in a subplot alongside the positive cloud) before `plt.show()`.
