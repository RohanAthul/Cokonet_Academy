# 🎵 App Store Music Reviews Analysis

## 📌 Project Overview
This project performs **Explatory Data Analysis (EDA)** and **Sentiment Analysis** on App Store music reviews to understand user behavior, satisfaction, and inconsistencies between ratings and textual feedback.

The analysis focuses on:
- Distribution of user ratings
- Review patterns across apps and countries
- Relationship between review length and ratings
- Sentiment analysis of review text
- Detection of **contradictory user behavior**

## 📂 Dataset
- File: `app_store_music_reviews.csv`
- Stored locally in the same directory as the notebook
- Source: Kaggle

This dataset contains 5,640 user-submitted reviews of major music streaming apps on iOS, gathered from the Apple App Store’s public RSS feed.

Covered applications include:

- Apple Music
- Spotify
- TIDAL
- SoundCloud
- Deezer
- Shazam

### Each review includes:
- 🌍 country: Reviewer’s regional App Store (e.g., US, GB, JP)
- 📝 review: Full text of the review (with title and body)
- ⭐ rating: User star rating from 1 to 5
- 🕓 date: Timestamp (ISO format)
- 👤 author: Reviewer display name  

These reviews were retrieved using Apple’s publicly accessible RSS feed endpoints. No login or scraping was used.

### Dataset Link and License
- https://www.kaggle.com/datasets/cluesec/app-store-music-app-reviews
- CC BY-NC-SA 4.0  

Credit to the dataset creator **cluesec** for making this data publicly available.

> Note: This project is for educational and analytical purposes only. All rights to the dataset belong to the original creator.

## ⚙️ Installation

Install dependencies using:

```bash
pip install pandas numpy matplotlib seaborn emoji scikit-learn langdetect nltk wordcloud
```

## 📚 Libraries Used

- pandas, numpy → Data manipulation

- matplotlib, seaborn → Visualization

- nltk (VADER) → Sentiment analysis

- sklearn → Text processing

- langdetect → Language detection

- wordcloud → Text visualization

- emoji → Emoji handling

## 🚀 How to Run

- Place dataset in project folder:

    - app_store_music_reviews.csv

- Launch Jupyter Notebook:

- Run all cells sequentially

## 🧹 Data Preprocessing
- Data Cleaning

    - Parsed date column into datetime format

    - Converted rating to integer

    - Checked dataset structure (shape, size, columns)
    - Checked missing values (count + percentage) and duplicate rows

- Feature Engineering

    - review_length → Length of each review

    - Year and Month → Extracted from date

## 📊 Exploratory Data Analysis (EDA)
⭐ Rating Distribution

- Count plot of ratings (1–5)
- Pie chart showing percentage share

📱 Ratings by App

- Stacked bar chart showing rating distribution per app

🌍 Ratings by Country

- Country-level rating breakdown using crosstab

✍️ Review Length Analysis

- Histogram of review lengths

- Boxplot: review length vs rating

## 😊 Sentiment Analysis

- Used NLTK VADER SentimentIntensityAnalyzer

- Generated a sentiment_score for each review:

```
data['sentiment_score'] = data['review'].apply(
    lambda x: sia.polarity_scores(str(x))['compound']
)
```

#### Score range:

+1 → Very positive

0 → Neutral

-1 → Very negative

## 📈 Sentiment Distribution

- Visualized using histogram with KDE

- Helps understand overall emotional tone of reviews

## ⚠️ Contradictory Review Analysis

A key analytical step in this project is identifying mismatches between ratings and sentiment.

#### Positive Ratings with Negative Sentiment

- Users giving 5 stars but writing negative reviews

```
(data['rating'] == 5) & (data['sentiment_score'] < -0.5)
```
-   👉 Possible reasons:

    - Sarcasm or irony

    - Misclick on rating

    - Poor sentiment detection in edge cases

#### Negative Ratings with Positive Sentiment

- Users giving 1 star but writing positive reviews
```
(data['rating'] == 1) & (data['sentiment_score'] > 0.5)
```
- 👉 Possible reasons:

    - Frustration unrelated to app quality

    - UI/UX issues but overall satisfaction

    - Incorrect rating selection

## 📊 Key Insights

### 1. Sentiment Distribution is Centered Around Neutral
- A large concentration of sentiment scores is clustered around **0 (neutral)**  
- This suggests many reviews are either:
  - Mixed in opinion, or  
  - Not strongly expressive in sentiment  
- VADER tends to assign neutral scores when text lacks strong emotional polarity

### 2. Positive Skew in Sentiment
- There is a noticeable **right skew toward positive sentiment (0.4 to 1.0)**  
- Indicates that a significant portion of users express **satisfaction in text reviews**
- Aligns with the general trend of higher ratings (4–5 stars)

### 3. Weakness in Multilingual Sentiment Detection
- Many contradictory reviews (especially in **German, Spanish, Italian, French**)  
  show **strong negative sentiment scores despite positive wording**

Example:
> “Ich finde die App super...” → scored highly negative

👉 This highlights a key limitation:
- VADER is optimized for English
- Performance drops significantly for non-English reviews

### 4. High Number of False Contradictions (5⭐ + Negative Sentiment)
- Multiple 5-star reviews show extremely negative sentiment scores (-0.8 to -0.96)
- Root cause:
  - Language mismatch (non-English text)
  - Misinterpretation of phrases by the sentiment model  

👉 Insight:
- These are not true contradictions, but probably model errors

### 5. “Happy Complaints” Reveal Real User Friction
- Some 1-star reviews have positive sentiment scores (> 0.5)
- These reviews often:
  - Use polite or neutral language  
  - Mention specific issues (e.g., pricing, bugs, missing features)

Example patterns:
- “It used to be great but stopped working...”
- “Please fix this feature...”

👉 Insight:
- Users may express frustration constructively rather than emotionally
- Sentiment alone is not sufficient to capture dissatisfaction

### 6. Ratings vs Sentiment ≠ Perfect Alignment
- While overall trends align:
  - High ratings → positive sentiment  
  - Low ratings → negative sentiment  
- There are systematic mismatches due to:
  - Language differences  
  - Subtle or complex phrasing  
  - Model limitations  


### 7. Key Analytical Takeaway
- Combining structured data (ratings) with unstructured text (reviews) is powerful  
- However:
  - Sentiment models must be chosen carefully  
  - Language filtering is critical in real-world datasets  

👉 Business implication:
- Relying purely on sentiment models without preprocessing can lead to misleading insights

## 📌 Future Improvements

- Perform text cleaning (stopwords, stemming, lemmatization)

- Generate word clouds for sentiment categories

- Filter reviews by language using langdetect

- Build a machine learning model to predict ratings

- Create an interactive dashboard (Power BI / Streamlit)