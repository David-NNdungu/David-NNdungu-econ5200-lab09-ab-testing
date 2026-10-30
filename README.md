# Diagnosing Experimental Failures in A/B Testing

## Objective
I examined how coding errors, assignment problems, marketplace interference, and repeated testing can affect the conclusions of an A/B test.

## Methodology
- I corrected three CUPED bugs involving the adjustment coefficient, covariate centering, and standard error calculation.
- I used chi-squared tests to check for sample ratio mismatch in two experiments.
- I compared marketplace outcomes with no interference, interference under individual randomization, and a full rollout.
- I used a Beta-Binomial model to calculate the probability that variant B was better and the expected loss of choosing B.
- I implemented a Bayesian A/B testing function and applied it to a longer experiment.
- I compared the Bayesian results with a two-sided permutation test.
- I simulated repeated testing to examine how peeking affects false positive rates.

## Key Findings

### CUPED
The original code reported 0.0% variance reduction. After my corrections, the variance reduction was 82.7%, and the standard error fell from 0.1539 to 0.0641. The adjusted treatment effect was 2.0185, close to the true effect of 2.0. All five CUPED checks passed.

### Sample Ratio Mismatch
In Experiment A, a 0.60 percentage point imbalance among 100,000 users produced a chi-squared statistic of 14.40 and a p-value of 0.000148.

In Experiment B, a 0.05 percentage point imbalance among 20 million users produced a larger chi-squared statistic of 20.00 and a p-value of approximately 0.00000774.

Both experiments triggered the 0.001 alarm. My conclusion was that I should examine the size and cause of the imbalance alongside its p-value. A smaller p-value does not necessarily mean a larger practical problem.

### Marketplace Interference
The naive treatment effect with interference was 0.1861, compared with 0.1428 for a full rollout. The overstatement was 23.3% of the naive estimate.

Control buyers were more sensitive to declining inventory, so their purchase rate fell further than the treated buyers’ rate. This widened the measured gap and made the discount look more effective than it was under a full rollout.

### Bayesian A/B Testing
With 3,000 users per group, the probability that B was better than A was 0.8866. The expected loss of choosing B was 0.000473.

The decision rule did not recommend shipping B because the probability did not exceed 0.95, even though the expected loss was below 0.001.

The permutation test produced a p-value of 0.2302, so I did not reject the null hypothesis of equal conversion rates at the 5% level.

### Longer Experiment
I applied my Bayesian function to an experiment with 12,000 users per group, with 1,428 conversions for A and 1,560 for B.

The probability that B was better increased to 0.9951, and the expected loss of choosing B fell to approximately 0.000007. Both conditions for shipping B were satisfied. The function passed the notebook’s checks.

### Repeated Testing
Across 1,000 experiments with no true treatment effect:
- Uncorrected peeking produced a 32.4% false positive rate.
- Bonferroni correction reduced the false positive rate to 1.9%.
- A single test at the planned end produced a 5.3% false positive rate.

My conclusion was that repeatedly checking results and stopping when a p-value becomes significant can create misleading findings. I would set the testing and stopping rules before examining the results.

## Repository Files
- `lab_ch09_diagnostic.ipynb`: My completed lab and extension, including explanations, outputs, and charts.
- `src/cuped_utils.py`: The CUPED utility provided in the lab, which I saved and checked against my corrected Part 1 results.
- `README.md`: My project overview and findings.

## Scope
I completed the required sections and the sequential testing extension. The optional, ungraded LaLonde challenge is not included in my analysis.
