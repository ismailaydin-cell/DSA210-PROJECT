
DSA 210 Fall/Spring Term Project: Sci-Fi & Fantasy Book Adaptations Box Office Performance Analysis

1. Motivation
The film industry frequently relies on pre-existing intellectual properties, particularly successful novels, to mitigate financial risks and guarantee an established fanbase. In the realms of Science Fiction and Fantasy, world-building and narrative depth are paramount, making books an incredibly rich source of material. 

The primary motivation behind this project is to quantitatively investigate whether the success and critical reception of a book on popular literary platforms (such as Goodreads) directly translate into financial success at the global box office. By understanding these dynamics, filmmakers, producers, and studios can make more data-driven decisions regarding which literary properties to acquire and how heavily to invest in their budgets.

 2. Data Source & Preparation
To address our research questions, two distinct datasets were integrated and synthesized:
1. **TMDB 5000 Movies Dataset:** Contains comprehensive metadata for nearly 5,000 movies, including global box office revenue, production budgets, titles, genres, and release dates.
2. **Goodreads Books Dataset:** Contains user-generated ratings, review counts, author details, and text reviews for thousands of books.

Data Cleaning and Integration Pipeline:
- Genre Filtering:** The TMDB dataset was strictly filtered to isolate movies belonging to the "Science Fiction" or "Fantasy" genres.
- Title Alignment Mapping:** Book titles and movie titles often have minor differences or structural discrepancies (e.g., *"Harry Potter and the Philosopher's Stone"* vs. *"Harry Potter and the Sorcerer's Stone"*). A custom mapping dictionary was implemented to ensure iconic flagship franchises matched perfectly between the two datasets.
- **Data Merging:** The datasets were combined using an inner join based on the aligned titles (`mapped_title` from movies and `book_title` from books).
- **Anomalies and Missing Values:** Records with missing values in target fields (`revenue`, `budget`, `book_rating`) were dropped. Furthermore, any films with unrecorded or erroneous values of `$0` for budget or revenue were rigorously filtered out to ensure statistical validity.

 3. Exploratory Data Analysis (EDA)
Our exploratory phase focused on understanding the core distributions and pairwise relationships within the synchronized data:
- **Box Office Revenue Distribution:** Visualized using a histogram with a Kernel Density Estimate (KDE). The distribution exhibits a heavy right-skew, indicating that while most adaptations achieve moderate success, a small handful of blockbuster franchises achieve astronomical box office returns.
- **Book Rating vs. Movie Revenue:** A scatter plot was utilized to visually inspect patterns between literary acclaim and commercial cinematic success. Preliminary visual inspection showed a clustering of data points around high book ratings (3.8 - 4.5 out of 5), with massive variance in the corresponding movie revenues.
- **Movie Budget vs. Movie Revenue:** A secondary scatter plot revealed a strong, positive, linear-like progression, suggesting that production budget is a highly influential factor in generating high box office revenue.

## 4. Hypothesis Testing
To mathematically evaluate the impact of book quality on cinematic revenue, we performed an **Independent Two-Sample T-Test** (with unequal variances assumed / Welch's T-Test).

* **Null Hypothesis ($H_0$):** The mean box office revenue of film adaptations originating from high-rated books ($\ge 4.0$ on Goodreads) is equal to the mean box office revenue of adaptations originating from lower-rated books ($< 4.0$).
* **Alternative Hypothesis ($H_1$):** The mean box office revenue of film adaptations originating from high-rated books is statistically significantly different from those originating from lower-rated books.

### Statistical Results:
* **Significance Level ($\alpha$):** 0.05
* **P-Value calculated by the model:** If $P < 0.05$, we successfully reject the null hypothesis. 

*Interpretation Note:* The system assesses whether critical literary acclaim acts as a financial catalyst, or if blockbusters succeed independently due to pure cinematic marketing and scale.

## 5. Machine Learning Methods
To move beyond basic statistical testing, we built predictive models to estimate a movie's box office revenue based on two primary features: `budget` and `book_rating`. The dataset was split into an 80% training set and a 20% testing set to prevent overfitting.

### Model 1: Linear Regression
- Used as a baseline interpretability model to evaluate linear trends.
- **Performance Evaluation:** Evaluated using the $R^2$ (R-squared) metric on the unseen test set, quantifying how much variance in box office revenue can be explained linearly by budget and book ratings.

### Model 2: Random Forest Regressor
- Implemented as a non-linear ensemble method to capture complex interactions (e.g., how high book ratings might amplify the return on a massive budget).
- **Configuration:** Utilized 100 decision trees with a fixed random state for full reproducibility.
- **Performance Evaluation:** Tested using the $R^2$ score and visualized via an *Actual vs. Predicted Revenue* scatter plot accompanied by a perfect prediction diagonal reference line.

## 6. Findings & Conclusions
1. **Budget is King:** Across all models, production budget remains the single strongest predictor of box office revenue in the Sci-Fi and Fantasy genres. High-concept worlds require significant capital, which translates heavily into visual appeal and market reach.
2. **The Literary Factor:** While a high book rating ensures an eager core audience, it does not act as a standalone guarantee for a billion-dollar box office hit. The interaction between budget allocation and the established intellectual property produces the most profound financial success.
3. **Model Superiority:** The Random Forest Regressor outperforms the basic Linear Regression model by effectively handling the right-skewed blockbuster outliers and capturing non-linear dynamics inherent in cinematic economics.

## 7. Limitations & Future Work
- **Sample Size Constraint:** The strict intersection of Sci-Fi/Fantasy movies that also have direct matches in the Goodreads dataset naturally restricts the final row count.
- **Omission of Marketing Data:** Production budgets are public, but global marketing and distribution expenses—which heavily dictate box office draw—are rarely disclosed and were omitted from this analysis.
- **Future Directions:** Future work could expand the scope by incorporating Natural Language Processing (NLP) sentiment analysis directly on the raw text of Goodreads book reviews, rather than relying solely on numerical star ratings.

---

## 8. AI Use and Disclosure
In accordance with the academic integrity guidelines of the DSA 210 course, the use of AI assistance is explicitly documented below:
- **AI Tool Used:** Gemini (Google)
- **Specific Tasks Assisted:** 1. Provided scaffolding and integration code to merge the `matplotlib` visualization scripts with the `scikit-learn` machine learning pipeline.
  2. Assisted in structuring and polishing the formal academic language of the final Markdown report text based on the project code outcomes.
- **Prompt Examples:** *"How do I merge a Linear Regression model and a Random Forest Regressor split into 80/20 train/test sets using budget and book_rating as features?"* and *"Help me structure my final report based on the DSA 210 guideline sections."*

---

## 9. Instructions to Reproduce This Analysis

To replicate the exploratory analysis, hypothesis testing, and machine learning models on your local machine, follow these steps:

### Prerequisites
Make sure you have Python 3.x installed on your system. 

### Step 1: Clone or Download the Repository
Ensure that you have all the repository files in a local directory.

### Step 2: Install Dependencies
Open your terminal or command prompt, navigate to the project folder, and run the following command to install the necessary libraries:
```bash
pip install -r requirements.txt
