
# Spam Email Classifier
### CSC 240 — Text Processing Project | West Chester University of Pennsylvania

A Java-based end-to-end text classification system that reads a real-world email dataset, extracts linguistic features, and classifies emails as **Spam** or **Not Spam** using a distance-based nearest-neighbor algorithm.

---

## Project Overview

This project simulates a real-world machine learning pipeline — without using any ML libraries. Every component is built from scratch in Java using core object-oriented design principles:

- **Read** a labeled email dataset from CSV
- **Extract** linguistic features from each email (word frequency, bigrams, statistics)
- **Model** spam and non-spam email profiles from training data
- **Classify** unseen emails by computing distance to each model
- **Output** predictions and save feature data to structured CSV files

---

## Features Extracted Per Email

| Feature | Description |
|---|---|
| Word frequency counts | How often each word appears in the email |
| Bigram counts | Frequency of two-word sequences |
| Total word count | Overall length of the email |
| Average word length | Statistical measure of word size |
| Unique word count | Vocabulary size of the email |

---

## Classification Approach

Two classification methods are implemented:

**1. Summary-Based Classification**
Computes a statistical summary (mean, min, max, standard deviation) for both spam and non-spam training sets. A new email is classified by computing its distance to each summary model — closest model wins.

**2. Nearest Neighbor Classification**
Compares a new email against every email in the training set. The majority class among the top `n` closest neighbors determines the prediction.

**Distance Metric:** Euclidean distance across all feature vectors.

---

## Project Structure

```
spam-classifier/
├── src/
│   ├── Email.java              # Represents a single email and its features
│   ├── FeatureExtractor.java   # Extracts features from raw email text
│   ├── EmailSummary.java       # Builds summary statistics for a group of emails
│   ├── Classifier.java         # Distance computation and classification logic
│   ├── CSVReader.java          # Reads and parses the dataset CSV
│   ├── CSVWriter.java          # Saves feature and summary data to CSV
│   └── Main.java               # Entry point — runs the full pipeline
├── data/
│   ├── spam_or_not_spam.csv    # Full Kaggle dataset (not included — see below)
│   ├── training.csv            # Training split with labels
│   └── test.csv                # Test split (no labels)
├── output/
│   ├── email_features.csv      # Per-email computed features
│   ├── summary_features.csv    # Group summary statistics
│   └── predictions.txt         # One prediction per line (Spam / Not Spam)
├── test/
│   └── ClassifierTest.java     # Unit tests for each requirement
└── README.md
```

---

## How to Run

### Prerequisites
- Java 11 or higher
- The dataset CSV from Kaggle (see Dataset section below)

### Setup

```bash
# Clone the repository
git clone https://github.com/carterfennen/spam-classifier.git
cd spam-classifier
```

### Compile

```bash
javac -d out src/*.java
```

### Run

```bash
java -cp out Main
```

The program will:
1. Read `data/training.csv` and extract features for all emails
2. Build spam and non-spam summary models
3. Read `data/test.csv` and classify each email
4. Write predictions to `output/predictions.txt`
5. Save all feature data to `output/email_features.csv` and `output/summary_features.csv`

---

## Dataset

This project uses the [Spam or Not Spam Dataset](https://www.kaggle.com/datasets/ozlerhakan/spam-or-not-spam-dataset) from Kaggle.

**To set up the dataset:**
1. Download the CSV from the link above
2. Place it in the `data/` directory as `spam_or_not_spam.csv`
3. The program will handle splitting into training and test sets automatically

The dataset is not included in this repository due to file size.

---

## Sample Output

**email_features.csv**
```
email_id, word_count, unique_words, avg_word_length, the_count, free_count, ...
1, 142, 87, 4.3, 5, 3, ...
2, 23, 19, 3.8, 0, 0, ...
```

**predictions.txt**
```
Spam
Not Spam
Not Spam
Spam
...
```

---

## Example — Classifying a Single Email

```
Input Email:
"Congratulations! You have won a FREE prize. Click here to claim your reward now."

Extracted Features:
  Word count:       14
  Unique words:     13
  Avg word length:  4.9
  'free' count:     1
  'win' count:      1

Distance to Spam Model:     2.14
Distance to Not Spam Model: 8.73

Prediction: SPAM
```

---

## Testing

Each requirement has at least one corresponding test in `test/ClassifierTest.java`.

```bash
javac -d out src/*.java test/*.java
java -cp out ClassifierTest
```

Tests cover:
- CSV reading and parsing
- Feature extraction accuracy
- Distance metric correctness
- Classification accuracy on known labeled data
- CSV output format validation

---

## Design Decisions

**Why Euclidean distance?**
It is well-suited for comparing numeric feature vectors of the same dimensionality and gives intuitive weight to differences across multiple features simultaneously.

**Why represent emails as objects (Email class)?**
Encapsulating each email's raw text, label, and computed features in a single class makes the system modular, testable, and easy to extend with new features without changing the classification logic.

**What would I add next?**
- TF-IDF weighting for more meaningful word frequency scoring
- A simple Naive Bayes classifier for comparison against nearest-neighbor accuracy
- A GUI dashboard for visualizing feature distributions between spam and non-spam groups
- Stop word filtering to remove common words (the, is, a) that don't differentiate spam

---

## Author

**Carter Fennen**
Computer Science — West Chester University of Pennsylvania
[GitHub](https://github.com/carterfennen) • carterfennen@icloud.com

---

## Course

CSC 240 — Text Processing | West Chester University of Pennsylvania
