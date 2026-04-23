# SMS Spam Detection

A machine learning model that classifies SMS messages as spam or not spam using Naive Bayes and NLP preprocessing.

---

## How It Works

1. Downloads the [SMS Spam Collection dataset](https://archive.ics.uci.edu/dataset/228/sms+spam+collection) from UCI
2. Preprocesses messages — lowercasing, removing special characters, stopword removal, and stemming
3. Trains a `MultinomialNB` classifier inside a `Pipeline` with `CountVectorizer` (unigrams + bigrams)
4. Tunes the smoothing parameter (`alpha`) via 5-fold cross-validated grid search, optimizing for F1
5. Saves the trained model with `joblib` for reuse

---

## Requirements

```bash
pip install nltk pandas numpy scikit-learn requests joblib
```

---

## Usage

### Train and save the model

```bash
python spam_detector.py
```

This will:
- Download and extract the dataset into `sms_spam_collection/`
- Train the model with grid search
- Save it as `spam_detection_model.joblib`
- Run predictions on 5 example messages

### Predict on custom messages

```python
from spam_detector import load_model, predict_messages

model = load_model("spam_detection_model.joblib")

messages = [
    "Win a free iPhone! Click here now.",
    "Can you pick up the kids today?"
]

predict_messages(model, messages)
```

**Sample output:**
```
Message: Win a free iPhone! Click here now.
Prediction: Spam
Spam Probability: 0.97
Not-Spam Probability: 0.03
--------------------------------------------------
Message: Can you pick up the kids today?
Prediction: Not-Spam
Spam Probability: 0.02
Not-Spam Probability: 0.98
--------------------------------------------------
```

---

## Model Details

| Component | Detail |
|---|---|
| Dataset | SMS Spam Collection (UCI) — 5,574 messages |
| Vectorizer | CountVectorizer, ngram range (1,2), max_df=0.9 |
| Classifier | Multinomial Naive Bayes |
| Tuning | GridSearchCV over alpha: [0.01 – 1.0], 5-fold CV |
| Metric | F1 Score |

---

## Project Structure

```
├── spam_detector.py               # Main script
├── spam_detection_model.joblib    # Saved model (generated after training)
└── sms_spam_collection/
    └── SMSSpamCollection          # Raw dataset (downloaded automatically)
```

---

## Dataset

SMS Spam Collection — Almeida, T. and Hidalgo, J. (2012). UCI Machine Learning Repository.  
https://archive.ics.uci.edu/dataset/228/sms+spam+collection
