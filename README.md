# econ3916-lab14-ols-diagnostics

# OLS Diagnostics: When the Standard Error Is Lying

## Objective
I wanted to see how violated regression assumptions affect the estimate, the standard error, or both, and why a regression can pass every diagnostic and still give a wrong answer.

## Methodology
- Simulated 2,000 observations with errors whose spread grows with x, then fitted one OLS regression with classical standard errors and with robust (HC3) standard errors.
- Ran 400 simulated samples and counted how often each 95% confidence interval contained the true slope, with a Monte Carlo standard error for each coverage rate.
- Built four residual patterns (well behaved, heteroskedastic, missing curvature, one outlier) and read each residual plot beside the Breusch-Pagan and Jarque-Bera tests.
- Computed leverage, standardised residuals and Cook's distance for the outlier panel, then deleted the outlier row and refitted.
- Fitted a California Housing regression with classical and HC3 standard errors, and checked the residuals against the 5.0 value ceiling.
- Wrote a function, `hc3_se(data, features, term)`, that returns the HC3 standard error of one coefficient, and used it with and without the 965 districts at the ceiling.
- Simulated a confounder `u` that drives both x and y, regressed y on x alone, and checked that the model passed Breusch-Pagan, Jarque-Bera and Cook's distance.

## Key Findings
- The classical and robust fits give the same slope, 2.0147. The standard errors differ: 0.0264 classical and 0.0290 robust. Heteroskedasticity affects the standard error, not the estimate.
- Over 400 samples, the classical interval covered the true slope in 91.8% of them (Monte Carlo SE 1.4%) and the robust interval in 93.5% (Monte Carlo SE 1.2%). The classical shortfall from 95% is about 2.4 Monte Carlo standard errors, which is more than chance. The robust one is about 1.2, which is within chance.
- Deleting one outlier out of 600 moved the slope by 0.58 of its own standard error (0.9%), but the standard error fell 33%. A single huge residual enters the error variance squared, so it affects the standard error far more than the slope.
- In California Housing, the HC3 standard error on AveRooms was 5.02 times the classical one.
- With `hc3_se()`, the HC3 standard error on AveRooms was 0.0121 with all districts and 0.0082 without the 965 at the 5.0 ceiling. The smaller number describes a different, selected sample, so it is not a reason to drop those districts.
- In the Challenge, a regression of y on x alone passed Breusch-Pagan (p = 0.661), Jarque-Bera (p = 0.769) and the Cook's distance check (max 0.0185), yet its slope was 2.713 against a true slope of 2.0. The diagnostics test the model's internal assumptions. They cannot detect an omitted confounder. For that I need a causal graph (DAG) and subject knowledge.
