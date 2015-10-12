# P7: Design an A/B Test

--------

Template Format
This template can be used to organize your answers to the final project. Items that should be copied from your answers to the quizzes should be given in blue.

## Experiment Design

Udacity courses currently have two options on the home page: "start free trial", and "access course materials".

If the student clicks "start free trial", they will be asked to enter their credit card
information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first.

If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated
fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to
continue enrolling in the free trial, or access the course materials for free instead.

![alt text](img/Screen.jpg)
<end>

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually
complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

### Metric Choice

The list below explains the use of each available metric as an invariant or evaluation metric.

(1) Number of cookies: That is, number of unique cookies to view the course overview page (dmin=3000)
**Invariant metric: YES** - it is used as initial unit of diversion to make sure that we split equally between control and experiment group 
**Evaluation metric: NO** - invariant metric should not change between tests (control, experiment)

(2) Number of userids: That is, number of users who enroll in the free trial (dmin=50)
**Invariant metric: NO** - this metric can not be used for diversion 
**Evaluation metric: NO** - the number depends on whether the students enrolls into the "start free-trail" or not. Only there we would get the userids

(3) Number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger) (dmin=240)
**Invariant metric: YES** - check that the same amount of users proceed from course page to the experiment page under controlled and experiment conditions 
**Evaluation metric: NO** - invariant metric should not change between tests (control, experiment)

(4) Click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page (dmin=0.01)
**Invariant metric: YES** - make sure the population of students is similar in both groups 
**Evaluation metric: NO** - invariant metric should not change between tests (control, experiment)

(5) Gross conversion: That is, number of userids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button (dmin= 0.01)
**Invariant metric: NO** - evaluation metric 
**Evaluation metric: YES** - the experiment might change this metric, since depending on their time availability the will be opted to a free version

(6) Retention: That is, number of userids to remain enrolled past the 14 day boundary (and thus make at least one payment) divided by number of userids to complete checkout (dmin=0.01)
**Invariant metric: NO** - not a good fit for invariant, depending on the experiment it could be used as evaluation metric 
**Evaluation metric: NO** - this metric is not relevant to test our hypothesis

(7) Net conversion: That is, number of userids to remain enrolled past the 14 day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial"
button (dmin= 0.0075)
**Invariant metric: NO** - evaluation metric 
**Evaluation metric: YES** - the experiment might change this metric, since depending on their time availability the will be opted to a free version

### Measuring Standard Deviation

I used the baseline values to analytically estimate the standard deviation (SD) of the 2 selected evaluation metrics (Gross conversion; Net conversion). Assuming a binomial distribution, SD = sqrt((p*(1-p)/N).

**Gross conversion:** N=400; p=0.2063; **SD=0.0202**

**Net conversion:** N=400; p=0.1093; **SD=0.0156**

A binomial distribution follows a normal distribution for large sample sizes. Both metrics (Gross- and Net conversion) depend on a number of unique cookies that click "start free trial" button, which is in line with our unit of diversion. This should lead to the assumption that the analytic estimate should be comparable to the empirical variability.

### Sizing
a) Number of Samples vs. Power (see "ProjectBaselineValues.xlsx" for detailed calculation)

Based on Beta, Alpha, Baseline conversion and minimum detectable effect I used a web calculator tool to calculate the sample size per branch (Tool used - [http://www.evanmiller.org/ab-testing/sample-size.html](http://www.evanmiller.org/ab-testing/sample-size.html "Sample Size Calculator")

![alt text](img/Capture.jpg)
<end>

To achieve sufficient power for both evaluation metrics we require a total number of page view of 685325. Bonferroni correction is not used since both evaluation metrics have to be valid and Bonferroni correction is "over" conservative for high correlation metrics.

b) Duration vs. Exposure
Based on the KPI "Unique cookies a day" = 40000 the experiments have to run 18 days. 

I do not consider the experiment as very risky for Udacity. At the end a part of the students get only an additional "self-assessment" question. The question itself is not ethically problematic. I do not assume that this experiment has an effect on the overall experience or functionality of the Udacity web-site. I would therefore divert all of the traffic to this experiment

### Experiment Analysis
###Sanity Checks

One of the first things to do once you finish collecting experimental data is to analyze the invariant metrics. I want to check if the experiment and control conditions are comparable. To achieve this we compare the numbers of "cookies" and "clicks" on the start free trail button across the experiment and control condition. Our assumption is that the split is 50%. Assuming a binomial distribution and a 95% confidence interval for the invariant metrics the below list shows the calculated results (for details see R-Studio file - "P7_AB_testing.Rmd")

Number of cookies: **Passed**
lower bound = 0.4988 / upper bound = 0.5012 / observed = 0.5006

Number of clicks on "Start free trail": **Passed**
lower bound = 0.4959 / upper bound = 0.5041 / observed = 0.5005

For both invariant metrics we are within the 95% confidence interval and hence we passed the sanity check.

### Result Analysis
a) Effect Size Tests
For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)

b) Sign Tests
For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)

c) Summary
State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.

### Recommendation
Make a recommendation and briefly describe your reasoning.

## Follow-Up Experiment
Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.

--------

Resources - list any sources you consulted to create your visualization:

- https://github.com/jsalminen/AB-testing
- https://rpubs.com/superseer/abtesting
- https://github.com/j450h1/P7-Design-an-A-B-Test
- http://www.evanmiller.org/ab-testing/sample-size.html
- ?

 
