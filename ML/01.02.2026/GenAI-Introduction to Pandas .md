# Notebook Summary: GenAI-Introduction to Pandas

## Cell 1 (markdown) — Section header

**Summary points**
- Introduces the notebook's topic: the Pandas framework.
- Serves as the title/heading for the entire notebook.

**Key Concepts**
- Pandas library overview

**Q&A**
- Q: What is this notebook about? A: Learning and demonstrating the Pandas data analysis library.

---

## Cell 2 (code) — Create a DataFrame from a dictionary

**Summary points**
- Builds a `dict` of `Name`, `Age`, `City` lists.
- Converts the dict into a `pandas.DataFrame` using `pd.DataFrame(data)`.
- Prints the resulting DataFrame to show its tabular structure.

**Key Concepts**
- DataFrame construction
- Dictionary-to-table conversion

**Q&A**
- Q: What does `pd.DataFrame(data)` do here? A: Converts a dictionary of equal-length lists into a tabular DataFrame, with dict keys becoming column names.
- Q: Why must the lists in `data` be the same length? A: Because each list becomes a column, and DataFrame rows are aligned by position across columns.

---

## Cell 3 (code) — Access a column

**Summary points**
- Selects the `Name` column using `df['Name']`.
- Demonstrates column access returns a `pandas.Series`.

**Key Concepts**
- Column selection / indexing

**Q&A**
- Q: What type is returned by `df['Name']`? A: A `pandas.Series`.

---

## Cell 4 (code) — Access a row by index

**Summary points**
- Uses `df.loc[0]` to retrieve the first row by label-based index.
- Shows that row access returns a Series indexed by column names.

**Key Concepts**
- Label-based indexing with `.loc`

**Q&A**
- Q: What does `df.loc[0]` return? A: The row at index label 0, as a Series with column names as its index.
- Q: How would `.iloc[0]` differ from `.loc[0]` here? A: `.iloc[0]` selects by integer position, which happens to give the same result since the default index is 0,1,2,...

---

## Cell 5 (code) — Add a new column

**Summary points**
- Adds a `Salary` column to `df` by assigning a list of values.
- Prints the updated DataFrame with four columns.

**Key Concepts**
- Adding/mutating DataFrame columns

**Q&A**
- Q: How is a new column added to an existing DataFrame? A: By assigning a list (or Series) to `df['NewColumn']`.

---

## Cell 6 (code) — Filter rows by condition

**Summary points**
- Filters `df` for rows where `Salary > 75000` using boolean indexing.
- Stores and prints the filtered subset as `high_salary`.

**Key Concepts**
- Boolean masking / conditional filtering

**Q&A**
- Q: What does `df[df['Salary'] > 75000]` return? A: A DataFrame containing only the rows where the Salary column value exceeds 75000.

---

## Cell 7 (code) — Compute a summary statistic

**Summary points**
- Calculates the mean of the `Age` column with `df['Age'].mean()`.
- Prints the average age of the dataset.

**Key Concepts**
- Descriptive statistics on a column

**Q&A**
- Q: What does `.mean()` compute? A: The arithmetic average of the numeric values in the column.

---

## Cell 8 (code) — Sort DataFrame by column

**Summary points**
- Sorts `df` by the `Age` column using `df.sort_values('Age')`.
- Stores the result in `sorted_df` and prints it.

**Key Concepts**
- Sorting DataFrames

**Q&A**
- Q: Does `sort_values` modify `df` in place? A: No, it returns a new sorted DataFrame by default (`inplace=False`).

---

## Cell 9 (code) — Select multiple columns

**Summary points**
- Selects the `Name` and `City` columns together using `df[['Name', 'City']]`.
- Demonstrates selecting a subset of columns as a new DataFrame.

**Key Concepts**
- Multi-column selection

**Q&A**
- Q: Why use double brackets `df[['Name', 'City']]`? A: The outer brackets index the DataFrame, and the inner list specifies multiple column names, returning a DataFrame rather than a Series.

---

## Cell 10 (code) — Group and aggregate data

**Summary points**
- Groups rows by `City` using `df.groupby('City')`.
- Calculates the mean `Salary` per city group.
- Prints the resulting grouped Series.

**Key Concepts**
- `groupby` aggregation

**Q&A**
- Q: What does `df.groupby('City')['Salary'].mean()` produce? A: A Series indexed by unique City values, showing the average Salary for each city.

---

## Cell 11 (code) — Create a DataFrame with missing values

**Summary points**
- Builds `data_with_nan` containing `None` values in `Age` and `Salary`.
- Creates `df_nan` from this dictionary to demonstrate missing data handling.
- Prints the DataFrame showing `NaN` entries.

**Key Concepts**
- Missing data representation (`None`/`NaN`)

**Q&A**
- Q: How does pandas represent the `None` values after DataFrame creation? A: As `NaN` (Not a Number) in numeric columns.

---

## Cell 12 (code) — Fill missing values

**Summary points**
- Uses `df_nan.fillna(...)` with a dict mapping columns to fill values.
- Fills missing `Age` with the column mean and missing `Salary` with the column mean.
- Prints the resulting DataFrame with no missing values.

**Key Concepts**
- Imputation via `fillna`

**Q&A**
- Q: Why pass a dict to `fillna`? A: To specify different fill values per column instead of a single fill value for the whole DataFrame.
- Q: What would happen if `Salary` mean was computed before filling `Age`? A: No effect since the two column computations are independent; each mean uses only its own column's non-null values.

---

## Cell 13 (code) — Drop missing values

**Summary points**
- Uses `df_nan.dropna()` to remove any row containing at least one `NaN`.
- Prints the resulting DataFrame with fewer rows.

**Key Concepts**
- Removing incomplete rows with `dropna`

**Q&A**
- Q: What is the trade-off of `dropna()` vs `fillna()`? A: `dropna()` discards potentially useful data (whole rows), while `fillna()` preserves rows but introduces estimated/imputed values.

---

## Cell 14 (code) — Empty cell

**Summary points**
- No content; placeholder cell.

---

## Cell 15 (code) — Create Pandas Series (list, custom index, dict)

**Summary points**
- Creates a `pd.Series` from a plain list with a default integer index.
- Creates another Series from the same list with a custom string index.
- Creates a Series directly from a dictionary (keys become the index).
- Accesses elements by integer position and by index label.

**Key Concepts**
- `pandas.Series` construction
- Default vs. custom index
- Series element access

**Q&A**
- Q: What becomes the index when a Series is created from a dictionary? A: The dictionary's keys.
- Q: How do `series[2]` and `series_with_index['b']` differ in access style? A: The first uses positional/default integer indexing; the second uses a custom label-based index.

---

## Cell 16 (code) — Series inspection methods

**Summary points**
- Uses `.head(3)` and `.tail(3)` to preview the first and last rows.
- Uses `.shape` to get the Series dimensions.
- Uses `.describe()` to generate descriptive statistics.
- Uses `.unique()` and `.nunique()` to inspect distinct values and their count.

**Key Concepts**
- Series exploration/inspection methods

**Q&A**
- Q: What does `.describe()` return for a numeric Series? A: Count, mean, std, min, quartiles, and max.
- Q: What's the difference between `.unique()` and `.nunique()`? A: `.unique()` returns the array of distinct values; `.nunique()` returns the count of distinct values.

---

## Cell 17 (code) — Series with missing values, count non-null

**Summary points**
- Creates a new Series containing a `None` among numeric values.
- Uses `.count()` to count only non-null/non-NA entries.

**Key Concepts**
- Null-aware counting

**Q&A**
- Q: Does `.count()` include the `None` value in its result? A: No, `.count()` counts only non-NA/non-null values.

---

## Cell 18 (code) — Comprehensive Series statistics and transformations

**Summary points**
- Computes `sum`, `mean`, `median`, `std`, `min`, `max` on the Series.
- Finds `idxmax`/`idxmin` — the index labels of the max/min values.
- Uses `value_counts()` to tally frequency of each value.
- Uses `isnull()`/`notnull()` to build boolean masks for missing data.
- Uses `fillna(0)` and `dropna()` to handle the missing value.
- Applies `apply(lambda x: x**2 if pd.notnull(x) else x)` to square elements while skipping nulls.
- Computes `cumsum()` and `cumprod()` for running totals/products.
- Sorts by `sort_values()` (by value) and `sort_index()` (by index).
- Uses `clip(lower=2, upper=8)` to cap values within a range.

**Key Concepts**
- Aggregate statistics
- Boolean null-checking
- Element-wise transformation with `apply`
- Cumulative operations
- Sorting
- Value clipping/bounding

**Q&A**
- Q: Why does the lambda in `apply` check `pd.notnull(x)`? A: To avoid squaring/propagating errors on the `None` entry and to just pass it through unchanged.
- Q: What does `clip(lower=2, upper=8)` do to values outside that range? A: Values below 2 are set to 2, and values above 8 are set to 8; values within range are unchanged.

---

## Cell 19 (code) — Querying/filtering a Series

**Summary points**
- Creates a Series from a dict of key-value pairs.
- Filters using comparison operators (`>`, `==`, `!=`) for boolean masking.
- Combines multiple conditions with `&` for compound filtering.
- Uses `.isin([...])` to select elements matching a list of values.
- Uses `.str.startswith('b')` on a string Series for text-based filtering.
- Uses `.loc[[...]]` for label-based selection and `.iloc[1:4]` for positional slicing.

**Key Concepts**
- Boolean indexing on Series
- Combined logical conditions
- String accessor methods (`.str`)
- Label vs. positional selection

**Q&A**
- Q: Why use `&` instead of `and` when combining `(series > 20)` and `(series < 50)`? A: Because `and`/`or` don't work element-wise on pandas objects; `&`/`|` perform vectorized boolean logic and require parentheses around each condition.
- Q: What does `.str.startswith('b')` require of the Series? A: The Series must contain string data to use the `.str` accessor.

---

## Cell 20 (code) — Load and analyze a housing dataset

**Summary points**
- Loads `Housing_Data.csv` into a DataFrame with `pd.read_csv`.
- Inspects the data using `.head()`, `.info()`, and `.describe()`.
- Computes mean, median, and standard deviation for selected numerical columns.
- Calculates pairwise correlation of several features with `price` using `.corr()`.
- Uses `.value_counts()` on categorical columns (e.g., `driveway`, `airco`) to see category frequencies.

**Key Concepts**
- Reading external CSV data
- Exploratory data analysis (EDA)
- Correlation analysis
- Categorical value distribution

**Q&A**
- Q: What does `df['lotsize'].corr(df['price'])` measure? A: The Pearson correlation coefficient between lot size and price, indicating the strength/direction of their linear relationship.
- Q: Why loop over `categorical_columns` for `value_counts()`? A: To efficiently print the frequency distribution of each categorical feature without repeating code per column.

---

## Cell 21 (code) — Placeholder cell

**Summary points**
- Contains only the comment `# Try this out`; no executable logic.
- Likely intended as a prompt for hands-on practice, not a functional example.

---

## Cell 22 (code) — Exercise: Sales data analysis with Series

**Summary points**
- Creates a `sales_series` of daily sales values indexed by day names.
- Accesses a single day's value (`Wednesday`) by label.
- Computes `sum()` for total weekly sales.
- Finds the highest/lowest sales days with `idxmax()`/`idxmin()`.
- Computes the weekly `mean()`.
- Filters days with sales above/below the average.
- Defines a ±20% threshold around the mean and filters "significantly different" sales days.

**Key Concepts**
- End-to-end Series-based analysis workflow
- Threshold-based outlier filtering

**Q&A**
- Q: How is the "significantly different" threshold defined in this exercise? A: As sales more than 20% above or below the average (`0.2 * average_sales`).
- Q: What would change if the threshold were increased to 50%? A: Fewer days would qualify as "significantly different" since the acceptable range around the mean would widen.

---

## Cell 23 (code) — Empty cell

**Summary points**
- No content; placeholder cell at the end of the notebook.

---

## Notebook-Level Review

**Overall Summary**
This notebook is an introductory tour of the Pandas library, progressing from DataFrame basics (creation, column/row access, adding columns, filtering, sorting, grouping) through handling missing data (`fillna`, `dropna`), into a deep dive on the `Series` object (construction, inspection, statistics, transformations, and querying). It closes with a real-world style exercise: loading and exploring a housing CSV dataset with correlation analysis, followed by a self-contained practice exercise analyzing weekly sales data with a Series. Together, the cells build from core syntax to applied, dataset-driven analysis.

**Concept Map**
- *DataFrame basics*: creation from dict, column/row selection (`[]`, `.loc`), adding columns, boolean filtering, sorting, multi-column selection
- *Aggregation*: `.mean()`, `.groupby()`
- *Missing data*: `NaN` representation, `.fillna()`, `.dropna()`
- *Series fundamentals*: construction from list/dict, custom index, element access
- *Series inspection*: `.head()`, `.tail()`, `.shape`, `.describe()`, `.unique()`, `.nunique()`, `.count()`
- *Series statistics*: `sum`, `mean`, `median`, `std`, `min`, `max`, `idxmax`, `idxmin`, `value_counts`
- *Series transformations*: `apply`, `cumsum`, `cumprod`, `sort_values`, `sort_index`, `clip`
- *Series querying*: boolean masks, compound conditions, `.isin()`, `.str` accessor, `.loc`/`.iloc`
- *Applied EDA*: `read_csv`, `.info()`, `.describe()`, `.corr()`, categorical `value_counts()`
- *Practice workflow*: building and analyzing a Series end-to-end with threshold-based filtering

**Mixed Q&A Quiz**
1. Q: Both DataFrames and Series support `.loc`/`.iloc`. What's the key difference in what each returns when used on a DataFrame vs. a Series? A: On a DataFrame, `.loc`/`.iloc` return a row (Series) or subset (DataFrame); on a Series, they return a scalar value or a sub-Series.
2. Q: How do the missing-data strategies in Cell 12/13 (DataFrame) relate to null-handling in Cell 18 (Series)? A: Both use the same core methods (`fillna`, `dropna`, `isnull`/`notnull`), showing these APIs are consistent across DataFrame and Series objects.
3. Q: In Cell 10, `groupby('City')['Salary'].mean()` aggregates a DataFrame; in Cell 22, similar aggregation (`mean()`) is applied directly to a Series. Why doesn't the sales exercise need `groupby`? A: Because `sales_series` has no repeating categorical grouping key — each day is unique, so a direct `.mean()` on the whole Series suffices.
4. Q: Cell 19 uses boolean indexing on a Series, and Cell 6 uses it on a DataFrame. What syntax do they share? A: Both use bracket indexing with a boolean condition, e.g., `series[series > 30]` and `df[df['Salary'] > 75000]`, following pandas' consistent boolean masking pattern.
5. Q: How could correlation analysis (Cell 20) be combined with the filtering techniques from Cell 6/19 to find high-value houses with strong feature correlation? A: You could first filter the DataFrame with boolean indexing (e.g., `df[df['bedrooms'] > 3]`) to isolate a subset, then recompute `.corr()` on that filtered subset to see if correlations with price change for that segment.
