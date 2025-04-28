# Final Project — Natural Language Processing  
## Twitter US Airline Sentiment Dataset

**Report and Analysis by Elham Ghiasi**

---

## Introduction

The Twitter US Airline Sentiment Dataset is a collection of tweets where people shared their thoughts about major U.S. airlines like United, American, Southwest, and Delta. With over 14,000 tweets, each one is labeled as having a positive, neutral, or negative sentiment.

This dataset provides insight into how customers feel about their airline experiences, especially when it comes to service. It’s often used in sentiment analysis projects and is a great resource for applying Natural Language Processing (NLP) in a real-world setting.

In this project:
- Perform exploratory data analysis (EDA)
- Clean and preprocess the text
- Create feature representations (Bag of Words and TF-IDF)
- Train simple models (Logistic Regression, Naive Bayes) and black-box models (LSTM, BERT)
- Evaluate and compare models

---

# Part 1: EDA & Feature Engineering

## Part 1.a — Dataset Overview

- Loaded dataset: shape (14640, 15)
- Selected columns: `text` and `airline_sentiment`
- No missing values found
- Imbalance detected: negative sentiment dominates

### Key Observations
- Negative tweets are the majority, followed by neutral and positive.
- Most tweets have lengths between 60–120 characters.
- Early identification of class imbalance is critical for fair model evaluation.
- Length distribution provides insight for preprocessing strategies.

## Part 1.b — Text Visualization

### Word Clouds per Sentiment
- **Negative tweets**: delayed, worst, cancelled, customer service
- **Neutral tweets**: flight, boarding, gate
- **Positive tweets**: thanks, great, helpful, love

### Most Frequent Words
- Common words: flight, time, delayed, service, customer, hours, wait, help, bag, lost, airport, cancelled, still, need, ticket
- Frequent airline-related issues are reflected.
- Cleaning required for noise words like "amp" and "ve".

### Tweet Length Histogram
- Top 20 most frequent words show strong focus on flight issues and customer service.
- Positive terms like "thanks" and "helpful" are less frequent.

## Part 1.c — Preprocessing & Feature Engineering

Performed:
- Lowercasing
- Removing URLs, mentions, punctuation, and numbers
- Removing stop words
- Lemmatization

Created two feature representations:
- **Bag of Words (BoW)**: (14640, 11487)
- **TF-IDF**: (14640, 11487)

BoW counts word occurrences; TF-IDF adjusts importance by frequency across documents. Both are ready for model training.

---

# Part 2: Model Building

Using TF-IDF for all simple model training. Models are evaluated with accuracy, precision, recall, F1-score, and confusion matrices.

## Part 2.a & 2.b — Simple Model Training

### Logistic Regression
- Accuracy: ~76–80%
- Handles sparse, high-dimensional data
- Easy to interpret
- Pipeline: Preprocessing → TF-IDF → Logistic Regression → Evaluation

### Multinomial Naive Bayes
- Accuracy: ~70–74%
- Lightweight and fast
- Struggles with neutral/positive confusion
- Pipeline: Preprocessing → TF-IDF → Naive Bayes → Evaluation

Both models provide solid baselines before moving to neural models.

## Black-box Model Training: LSTM & BERT

### LSTM Model
- Accuracy: ~78–82%
- Better balance across classes
- Handles sequential context
- Pipeline: Preprocessing → Tokenization → Embedding → LSTM → Dense Layer
- Architecture: Embedding(10000 vocab, 64 dims) → LSTM(128 units) → Dropout(0.5) → Dense

### BERT Model
- Accuracy: 85%
- Macro F1-score: 0.80
- Best overall performance
- High precision/recall on negative and positive classes
- Pipeline: Raw Tweets → WordPiece Tokenizer → BERT Encoder → Dense Layer

BERT, pre-trained on a large corpus, understands deep contextual relationships, outperforming all other models.

---

# Part 2.c — Model Comparison

| Model                  | Accuracy | Macro F1-score |
|-------------------------|----------|---------------|
| Logistic Regression     | 76–80%   | Moderate       |
| Naive Bayes             | 70–74%   | Lower          |
| LSTM                    | 78–82%   | Good           |
| BERT                    | 85%      | Excellent      |

Summary:
- BERT outperformed all models significantly.
- LSTM followed closely.
- Logistic Regression was impressive for its simplicity.
- Naive Bayes lagged behind.

---

# Part 3: Interpretation, Error Analysis & Trade-Offs

## Part 3.a — Error Analysis

### Misclassified Examples
- Sarcastic tweets misinterpreted (e.g., sarcastic compliments).
- Short tweets lacked context.
- Neutral tweets often confused with negative/positive.

### Patterns Found
- Sarcasm detection is challenging.
- Vague, short tweets led to misclassification.
- Passive-aggressive tones were often misinterpreted.

## Part 3.b — Class Sensitivity & Confusion Patterns

### Observations
- Negative class had best precision/recall due to volume.
- Neutral tweets were hardest to classify.
- Positive class was relatively stable.

Confusion matrices revealed frequent misclassification between neutral and negative.

## Part 3.c — Explainability vs. Performance

| Model Type      | Pros                                           | Cons                                     |
|-----------------|------------------------------------------------|-----------------------------------------|
| Simple Models   | Fast, interpretable, easy to debug             | Miss subtlety, weak on sarcasm, limited context |
| Black-box Models| High accuracy, capture nuanced language        | Expensive to train, hard to explain      |

BERT provided superior performance at the cost of interpretability and computational resources.

---

# Conclusion

While simple models like Logistic Regression are fast and interpretable, deep models like BERT are far superior in capturing real-world language complexity.  

The best model choice depends on project goals:  
- **Simple models** for quick, interpretable applications.
- **Deep models** like **BERT** when accuracy is critical despite complexity and cost.
