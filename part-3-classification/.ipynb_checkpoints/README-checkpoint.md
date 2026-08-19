# Classification Model Selection

### 1. Confusion Matrix and Accuracy Score

<table>
  <thead>
    <tr>
      <th></th>
      <th colspan="3">Prediction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">Actual</th>
      <th></th>
      <th>Negative (-ve)</th>
      <th>Positive (+ve)</th>
    </tr>
    <tr>
      <td>Negative (-ve)</td>
      <td>True Negative (-ve)</td>
      <td>False Postive (+ve)</td>
    </tr>
    <tr>
      <td>Positive (+ve)</td>
      <td>False Negative (-ve)</td>
      <td>True Positive (+ve)</td>
    </tr>
  </tbody>
</table>

- False Positive: Type I Error
  It is not much dangerous. For example if patient go for diagnosis. And result came out for suffering from some diseases then its false positive he can test in another hospital to confirm the result and based on that doctor can recommend him some medicine to take or not.
- False Negative: Type II Error
  It is kind of dangerous. For example if patient go for diagnosis. And result came out normal even if he is suffering from diseases then he might think everything is alright and might not check again and doctor might not recomment him any medicine it can be harmful. So it is False Negative

**For Example:** Taking 100 patient

<table>
  <thead>
    <tr>
      <th></th>
      <th colspan="3">Prediction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">Actual</th>
      <th></th>
      <th>Negative (-ve)</th>
      <th>Positive (+ve)</th>
    </tr>
    <tr>
      <td>Negative (-ve)</td>
      <td>43</td>
      <td>12</td>
    </tr>
    <tr>
      <td>Positive (+ve)</td>
      <td>4</td>
      <td>41</td>
    </tr>
  </tbody>
</table>

*Accuracy Rate (AR) and Error Rate (ER)* can be calculated as:

**Accuracy Rate (AR)**
- `AR = Correct/Total`
- `(TN + TP)/Total`
- `(43 + 41)/100`
- `84%`

**Error Rate (ER)**
- `ER = Incorrect/Total`
- `(FP + FN)/Total`
- `(4 + 12)/100`
- `16%`

#### Steps
1. How to choose the Right Classification Algorithm for your dataset
2. Optimizing Model Selection: Streamlined classification code in python
3. Evaluating classification algorithm: accuracy metrics in python
4. Model Selection process: Evaluating classification

# Evaluating classification models performance

A beginner-friendly guide to four core ideas in evaluating classification models: logistic regression, the accuracy paradox, CAP curves, and CAP-based accuracy ratio.

---

## 1. Logistic regression: interpreting predictions and error

### Simple explanation

Logistic regression is used when you want to predict one of two outcomes, for example spam or not spam, fraud or not fraud. Instead of predicting a straight number like linear regression, it predicts a **probability** between 0 and 1, using a curve called the sigmoid function.

```
P(y=1) = 1 / (1 + e^(-z))
where z = b0 + b1*x1 + b2*x2 + ...
```

The straight line output `z` (called the linear combination of your inputs) gets squeezed into the 0 to 1 range by the sigmoid, so it can be read as a probability.

### Key concepts

- **Sigmoid function**: converts any number into a value between 0 and 1
- **Decision threshold**: the cutoff (usually 0.5) that turns a probability into a class label
- **Type I error (false positive)**: model predicts positive, but actual is negative
- **Type II error (false negative)**: model predicts negative, but actual is positive

### Example

Say a model predicts the probability that an email is spam:

| Email | Predicted probability | Threshold 0.5 | Predicted label |
|---|---|---|---|
| A | 0.82 | above | Spam |
| B | 0.35 | below | Not spam |
| C | 0.51 | above | Spam |

If email C was actually not spam, that is a **false positive** (Type I error). If email B was actually spam, that is a **false negative** (Type II error).

### Common mistakes

- Treating the output as a hard 0/1 label from the start, instead of a probability you threshold yourself
- Always using 0.5 as the threshold, even when false positives and false negatives have very different costs (for example, in fraud detection you might lower the threshold to catch more fraud, accepting more false alarms)

---

## 2. Machine learning model evaluation: accuracy paradox and better metrics

### Simple explanation

**Accuracy** is the percentage of predictions the model got right:

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

It sounds like the obvious metric to use, but it can be badly misleading when your classes are **imbalanced** (one class is much more common than the other). This misleading effect is called the **accuracy paradox**.

### Example: the accuracy paradox

Imagine a disease that only 1% of patients actually have. A lazy model that predicts "no disease" for everyone will be right 99% of the time.

| Actual | Predicted "no disease" always |
|---|---|
| 990 healthy patients | 990 correct |
| 10 sick patients | 0 correct (all missed) |

```
Accuracy = 990 / 1000 = 99%
```

99% accuracy looks great on paper, but the model is medically useless. It never catches a single sick patient.

### Better metrics

| Metric | Formula | What it tells you |
|---|---|---|
| Precision | TP / (TP + FP) | Of everything predicted positive, how much was actually positive |
| Recall (sensitivity) | TP / (TP + FN) | Of everything actually positive, how much did the model catch |
| F1 score | 2 * (Precision * Recall) / (Precision + Recall) | Balance between precision and recall |
| ROC-AUC | area under the ROC curve | How well the model separates the two classes across all thresholds |

### Common mistakes

- Reporting accuracy alone on an imbalanced dataset (like fraud, disease, or rare-event detection) without checking precision and recall
- Not deciding upfront whether missing a positive case (low recall) or raising false alarms (low precision) is more costly for the problem

---

## 3. CAP curves: assessing model performance

### Simple explanation

A **CAP curve** (Cumulative Accuracy Profile) shows how quickly your model finds the actual positive cases once you sort customers, patients, or transactions by the model's predicted probability, from most likely positive to least likely.

You plot three lines on the same graph:

- **Random model line**: a straight diagonal line. If you picked people at random, you would find positives at a steady, boring rate.
- **Perfect model line**: shoots straight up to 100% almost immediately, because a perfect model would rank every real positive case first.
- **Your model's CAP curve**: sits somewhere between random and perfect. The closer it hugs the perfect line, the better your model is.

### Example

Say you have 1,000 customers and 100 of them will actually churn (leave). You rank all 1,000 customers by the model's predicted churn probability, then walk down the list:

- After contacting the top 10% of customers (ranked by risk), a good model might have already found 60 of the 100 churners
- A random approach would only find about 10 churners in that same top 10%

Plotting "% of customers contacted" (x-axis) against "% of actual churners found" (y-axis) gives you the CAP curve. A curve that reaches 60% churners found at just 10% of customers contacted is clearly beating random guessing by a wide margin.

### Common mistakes

- Confusing the CAP curve with the ROC curve. They look similar but plot different things: ROC plots true positive rate against false positive rate, while CAP plots cumulative positives found against the percentage of the population targeted.
- Reading only the shape of the curve without quantifying "how good" with a number (see Accuracy Ratio below).

---

## 4. CAP analysis: assessing a classification model with accuracy ratio

### Simple explanation

The **Accuracy Ratio (AR)**, also called the CAP ratio, turns the CAP curve into a single number, so you can compare models without eyeballing graphs.

```
AR = Area between your model's curve and the random line
     -----------------------------------------------------
     Area between the perfect model's curve and the random line
```

### Interpreting the value

| Accuracy ratio | What it means |
|---|---|
| Close to 0 | Model is barely better than random guessing |
| Close to 1 | Model is close to the perfect model |
| 0.6 to 0.9 | Common range for a genuinely useful, well-performing model in practice |

### Example

Continuing the churn example: if the area between your model's curve and the random line is 30 (in whatever unit the chart uses), and the area between the perfect model and the random line is 40, then:

```
AR = 30 / 40 = 0.75
```

An AR of 0.75 tells you, in one number, that your model captures 75% of the theoretical maximum improvement over random guessing. That is a strong, usable model.

There is also a quick rule of thumb some practitioners use, based on where the CAP curve sits at the 50% mark of the population:

| CAP value at 50% | Model quality |
|---|---|
| below 60% | poor |
| 60% to 70% | okay |
| 70% to 80% | good |
| 80% to 90% | very good |
| above 90% | too good, check for data leakage |

### Common mistakes

- Treating a very high AR (above 90%) as automatically great. In practice, it often means the model is accidentally using information it should not have access to (data leakage), such as a feature that indirectly reveals the answer.
- Comparing accuracy ratios across two datasets that have very different class balance. AR is best used to compare models on the *same* dataset.

---

## Quick summary

| Concept | One-line takeaway |
|---|---|
| Logistic regression | Predicts a probability via the sigmoid function, then a threshold turns it into a class |
| Accuracy paradox | High accuracy can hide a useless model on imbalanced data |
| CAP curve | Shows how fast your model finds real positives compared to random guessing |
| Accuracy ratio | Compresses the CAP curve into a single 0 to 1 score for easy comparison |