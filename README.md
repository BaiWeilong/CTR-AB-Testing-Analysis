# CTR A/B Testing Analysis

## Project Overview

This project involves analyzing an A/B test conducted to compare the performance of two groups (control and test) in terms of their Click-Through Rate (CTR). The test evaluates whether a new algorithm implementation improves user engagement and interaction.

---

## Tech Stack

- **Python**: For data analysis and visualization.
- **Pandas**: For data manipulation.
- **Seaborn & Matplotlib**: For visualization.
- **SciPy**: For statistical testing.
- **ClickHouse**: As the data storage and querying platform.

---

## Logic and Solution Approach

### 1. Data Collection and Preparation

- Data was retrieved from ClickHouse using SQL queries.
- The dataset includes the following columns:
  - `exp_group`: Experiment group (1 for control, 2 for test).
  - `user_id`: Unique identifier for each user.
  - `likes`: Number of likes received.
  - `views`: Number of views recorded.
  - `ctr`: Click-Through Rate (calculated as likes/views).

### 2. Exploratory Data Analysis (EDA)

- Checked data distribution and ensured no missing values.
- Key statistics:
  - Group 1 (Control): 10,020 users.
  - Group 2 (Test): 9,877 users.
- Histogram and descriptive statistics revealed non-normal distributions for `ctr`.

### 3. Statistical Testing

#### Tests Performed:
1. **Shapiro-Wilk Test**:
   - Null Hypothesis (H₀): Data is normally distributed.
   - Result: p-value = 0.0 → Reject H₀ (data is not normally distributed).

2. **Levene's Test**:
   - Null Hypothesis (H₀): Equal variances between groups.
   - Result: p-value = 0.0 → Reject H₀ (variances are not equal).

3. **Welch's T-Test**:
   - Null Hypothesis (H₀): Mean CTR is the same for both groups.
   - Result: p-value = 0.685 → Fail to reject H₀ (no significant difference in mean CTR).

4. **Mann-Whitney U Test**:
   - Null Hypothesis (H₀): CTR distributions are the same.
   - Result: p-value < 0.05 → Reject H₀ (distributions differ significantly).

5. **Smoothed CTR Testing**:
   - Applied a smoothing formula to adjust CTR for low-volume users.
   - Welch's T-Test on smoothed CTR: p-value = 0.051 (not statistically significant).

6. **Poisson Bootstrap**:
   - Bootstrapping revealed a statistically significant difference in CTR favoring the control group.

7. **Bucketized T-Test**:
   - Created buckets of users for aggregated CTR analysis.
   - Result: Control group had a higher bucketized CTR than the test group.

---

## Results

1. **CTR Comparison**:
   - Control Group: Higher average CTR across multiple statistical tests.
   - Test Group: Lower CTR, indicating the new algorithm may not perform better.

2. **Statistical Significance**:
   - Non-parametric tests (Mann-Whitney U, Poisson Bootstrap) consistently showed significant differences.

---

## Recommendations

1. **Do Not Roll Out the New Algorithm Immediately**:
   - The test group underperformed in CTR compared to the control group.

2. **Investigate Algorithm Performance**:
   - Conduct additional analysis to identify potential issues or subgroups where the algorithm may be effective.

3. **Focus on Specific User Segments**:
   - Tailor algorithm deployment for user segments where it shows promise.

4. **Plan Further A/B Testing**:
   - Refine the algorithm and retest to confirm improvements before a full-scale rollout.

---

## Future Steps

- Analyze the impact of the new algorithm on other metrics like user retention and satisfaction.
- Perform qualitative research to understand user behavior changes under the new algorithm.
- Explore alternative algorithm enhancements to improve CTR.
