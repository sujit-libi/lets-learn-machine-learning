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