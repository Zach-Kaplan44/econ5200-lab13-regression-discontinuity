# econ5200-lab13-regression-discontinuity
# Instrumental Variables: Weak Instruments, Compliers and the Exclusion Restriction

## Objective
I set out to see how instrumental variables estimates behave when the instrument is weak and when the exclusion restriction does not quite hold, and to confirm that I can reproduce two-stage least squares from its component regressions.

## Methodology
- Simulated data with a known true effect of 1.0 and an endogenous regressor that biases OLS upward, then estimated the effect by both OLS and 2SLS across repeated draws.
- Tuned the instrument's strength so the first stage was weak, recording the first-stage F statistic alongside each estimate and summarizing both by their medians.
- Tracked how often 2SLS returned a statistically significant estimate and how far those significant estimates sat from the true value of 1.0.
- Replicated Card's (1995) college-proximity instrument for years of schooling, estimating the return to schooling by OLS and by 2SLS and reporting the first-stage F statistic.
- Violated the exclusion restriction on purpose by letting the instrument have a small direct effect on the outcome, then measured how far the 2SLS estimate moved as a result.
- Wrote my own 2SLS function as two chained OLS regressions, fitting the endogenous regressor on the instrument and controls, then the outcome on the fitted values, and checked the coefficients against the `linearmodels` implementation.

## Key Findings
- With a weak instrument, 2SLS was no rescue. At a median first-stage F of 0.5, the median 2SLS estimate was 2.038 against a median OLS estimate of 1.998, so 2SLS sat slightly further from the true effect of 1.0 than the biased OLS estimate it was meant to fix.
- When a weak-instrument 2SLS estimate did come back significant, it was usually far from the truth, so significance was not a sign that the estimate was any good.
- In the Card (1995) replication the instrument was strong enough to work with, with a first-stage F of 16.7. The OLS return to schooling was 0.0740 and the 2SLS return was 0.1323, so instrumenting raised the estimated return rather than lowering it.
- A small direct effect of the instrument on the outcome shifted the 2SLS estimate by a visible amount, which showed me that the exclusion restriction has to hold tightly and cannot be tested from the data.
- My hand-built 2SLS function matched the `linearmodels` coefficients, which confirmed that the estimator is just two OLS regressions stacked. The standard errors are where the hand-built version needs care, since the second stage treats the fitted values as if they were observed.
