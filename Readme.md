# Credit Card Launch Analysis

This document outlines the steps performed during the credit card launch analysis, focusing on data cleaning, processing, and visualization techniques using Python's Pandas library.

## Basic Operations

# Phase 1

1. **Pandas Operations**
   - **Functions Used**: `head()`, `shape`, `info()`, `describe()`
   - **Customer Analysis**: Annual income overview

2. **Check and Replace Null Values**
   - Identify null values using:
     ```python
     df['a'].isnull().sum()
     ```
   - Replace null values with either the **mean** or **median**, depending on the presence of outliers and the data distribution.
   - Fill missing values based on **Occupation**.

3. **Outlier Treatment for Income**
   - Set minimum annual income threshold at **100**. Any values below 100 are considered outliers.
   - Replace outliers with the **median** income based on the **Occupation**.

4. **Age Treatment**
   - **Check for Null Values** in the age column.
   - Ensure age is between **15** (inclusive) and **80** (inclusive).
   - Fill outliers with the **median age** based on **Occupation**.

5. **Visualization of Age, Gender, and Location**
   - Define age bins:
     - **Youngsters**: 18 to 25
     - **Mid-age Professionals**: 26 to 48
     - **Seniors**: 49 to 65
   - Create the following charts:
     1. Location vs Gender vs Count
     2. Age Group Contribution

### Credit Data Operations

6. **Identify and Drop Duplicates**
   - Find duplicates between customer data and credit data and drop them using:
     ```python
     df.drop_duplicates(subset='', keep='last', inplace=True)
     ```

7. **Null Value Treatment for Credit Score**
   - Check for null values in the credit score column.
   - Categorize credit scores using the following bins:
     ```python
     bins = [299, 450, 500, 550, 600, 650, 700, 750, 800, 850]
     ```
   - Replace null values with the most frequent value from each credit score group.

8. **Outstanding Amount Analysis**
   - Check if there are any null values in the outstanding amount column.
   - Ensure the outstanding amount does not exceed the credit limit. Use the `describe()` method for validation.
   - Replace outliers in the outstanding amount with the credit limit if they exceed it.

9. **Correlation Analysis**
   - Calculate the correlation between credit score and credit limit:
     ```python
     df_merge[['credit_score', 'credit_limit']].corr()
     ```

### Transaction Data Operations

10. **Null Value Treatment in Transactions**
    - Check for null values using the `describe()` method.
    - Fill null values based on the mode of the product category.
    - Ensure transaction amounts are not zero:
      - If any are zero, fill with the mean or median based on platform, category, and payment type.
      - Standardize common fields (e.g., "Amazon", "Electronic", "Credit card") by stripping whitespace.
      - Use median only for non-zero values and replace them accordingly.

11. **Outlier Treatment using IQR**
    - Calculate IQR for transaction amounts:
      ```python
      q1, q3 = df_transactions['tran_amount'].quantile([0.25, 0.75])
      IQR = q3 - q1
      lower_range = q1 - 2 * IQR
      upper_range = q3 + 2 * IQR
      print(lower_range, upper_range)
      ```
    - Fill the transaction amounts with the mean based on the product category level.

### Visualizations 

![image](https://github.com/user-attachments/assets/99c2811a-3dde-475f-a517-2e246869b952)

![image](https://github.com/user-attachments/assets/22a15196-2125-40f7-9c81-3a357fc9bf71)

![image](https://github.com/user-attachments/assets/3c4705f2-ec8c-43c8-a140-65d44a81460a)

![image](https://github.com/user-attachments/assets/40066ed3-6a7b-4592-be37-2a963cb6cfff)

![image](https://github.com/user-attachments/assets/4981f950-bb51-4123-9717-cdba69bdbf4e)

![image](https://github.com/user-attachments/assets/0ffb5154-9e4c-4018-9753-2ea3206b60f7)

# Phase 2

In this phase, we will focus on advanced analytics and model building to further assess the effectiveness of the new credit card and its impact on customer behavior and transaction trends. Detailed methodologies will be outlined in subsequent sections.

### A/B Testing

A/B testing was conducted on server data to evaluate the impact of different strategies on customer engagement. The following hypotheses were tested:
- **Null Hypothesis (H0)**: The new credit does not make the transaction better than the old credit.
- **Alternative Hypothesis (H1)**: The new credit makes the transaction better than the old credit.

Sample size derivation was decided using different effect sizes:
```python
effective_sizes = [0.1, 0.2, 0.3, 0.4, 0.5, 1]
for i in effective_sizes:
    result = sms.tt_ind_solve_power(
        effect_size=i,
        alpha=alpha,
        power=power,
        ratio=1,
        alternative="two-sided"
    )
    print(f'Effective size {i} and sample size is {result}')
```
### Conclusion
 - Based on the A/B testing results, we observed that the Z-score exceeded the Z-critical value, providing sufficient evidence to reject the null hypothesis. Consequently, we can conclude that the new credit card offers a better transaction experience compared to the old credit card.
  We are pleased to recommend the launch of the new credit card based on these findings.
