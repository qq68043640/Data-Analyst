# A/B testing Project at Udacity ---Free Trial Screener
by Kun Hu 

## Experiment Overview: Free Trial Screener
Udacity courses have two options on the home page:"start free trial" and "access course materials".If the student clicks "start free trial",
they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks"access course materials",they will be able to view the videos and take the quizzes for free,but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked
"start free trial", they were asked how much time they had available to
devote to the course. If the student indicated 5 or more hours per week,
they would be taken through the checkout process as usual. If they indicated
fewer than 5 hours per week, a message would appear indicating that
Udacity courses usually require a greater time commitment for successful
completion, and suggesting that the student might like to access the course
materials for free. At this point, the student would have the option to
continue enrolling in the free trial, or access the course materials for free
instead. This [screenshot](https://drive.google.com/file/d/0ByAfiG8HpNUMakVrS0s4cGN2TjQ/view) shows what the experiment looks like.

The hypothesis was that this might set clearer expectations for students
upfront, thus reducing the number of frustrated students who left the free
trial because they didn't have enough time—without significantly reducing
the number of students to continue past the free trial and eventually
complete the course. If this hypothesis held true, Udacity could improve the
overall student experience and improve coaches' capacity to support
students who are likely to complete the course.

The unit of diversion is a cookie, although if the student enrolls in the free
trial, they are tracked by user-id from that point forward. The same user-id
cannot enroll in the free trial twice. For users that do not enroll, their user-id
is not tracked in the experiment, even if they were signed in when they
visited the course overview page.


## 1.Experiment Design
### 1.1 Metric Choice
`Here is a list of metrics we care about:`
* Invariant Metrics: Number of cookies, Number of clicks, Click-through-probability
* Evaluation Metrics: Gross conversion, Retention,Net Conversion.

`Invariant metrics`  
* **Number of cookies**(*Number of unique cookies to view the course overvirew page*):Should be evenly distributed within control and experiment group.
* **Number of clicks**(*Number of unique cookies to click the "Start free trial button"*): This should also happens before the free trail screener is triggered.
* **Click-through-probability**(*Number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page*):Happens before users see the changes.So this is invatiant metric.

`Evaluation metrics`
* **Number of user-ids**(*number of users who enroll in the free trial*): This metric are affected by the experiment change, so it's an evaluation metric.
* **Gross conversion**(*Number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button*): Since user-ids is evaluation metric and number of clicks is not, so gross conversion is evaluation metric, we only need to pick one beween number of user-ids or gross conversion .In this experiment, I use gross conversion as evaluation metric. 
* **Retention**(*Number of user-ids to remain enrolled past the 14-day boundary divided by number of user of user-ids to complete checkout*):we expect retention will be signifisant different between two groups 
* **Net conversion**(*Number of user-ids to remain enrolled pass the 14-day bountry divided by the number of unique cookies to click the "Start free trial" button*):we expect net conversion.

### 1.2 Measuring Viariability

Before we confuct the experiment, Udacity collected daily values for unique cookies, enrollments,click-through-probability,gross conversion, retention, net conversion as baseline data. 

Baseline table：

| Title                                              | Measure |
|-----------------------------------------------------|-----------|
| Unique cookies to view page per day:                | 40000     |
| Unique cookies to click "Start free trial" per day: | 3200      |
| Enrollments per day:                                | 660       |
| Click-through-probability on "Start free trial":    | 0.08      |
| Probability of enrolling, given click:              | 0.20625   |
| Probability of payment, given enroll:               | 0.53      |
| Probability of payment, given click:                | 0.1093125 |

We preditct we need approximately 5000 cookies per day in each group.
Baed on table above,we can have calculation results:

| Evaluation         | Gross conversion | Retension | Net conversion |
|--------------------|------------------|-----------|----------------|
| Standard Deviation | 0.0202           | 0.0549    | 0.0156         |

### 1.3 Sizing
#### 1.3.1 Number of samples vs Power

In this experiment, we use use alpha of 0.05 and beta of 0.2.
we use minimum detactable table as follows:

| Minimum detectable effect: | Measure: |
|----------------------------|----------|
| Number of cookies         | 3000     |
| Number of user-ids       | 50       |
| Number of clicks         | 240      |
| Click-through-probability | 0.01     |
| Gross conversion        | 0.01     |
| Retention           | 0.01     |
| Net conversion          | 0.0075   |

By using [calculator](http://www.evanmiller.org/ab-testing/sample-size.html),we can have sample results:

| Evaluation metrics: | d(min) | Baseline conversion rate | Sample Size  | Total Sample size  | Pageview required  |
|---------------------|--------|--------------------------|--------------|--------------------|--------------------|
| Gross conversion:   | 0.01   | 20.625%                  | 25835        | 51670              | 645875             |
| Retention:          | 0.01   | 53%                      | 39155        | 78310              | 4741212            |
| Net conversion:     | 240    | 10.93125%                | 27413        | 54826              | 685325             |

#### 1.3.2 Choosing Duration and Exposure
We split 50% traffic to control group and 50% experiment control.Assume we have 40000 pageviews per day,if we pick retention, the experiment would take 4741212/40000=119 days.Instead, if we pick gross retention, we only take 645875/40000=17 days,which is more practical time frame for our experiment.

## 2.Analysis

The experimental data can be found [here](https://docs.google.com/spreadsheets/d/1Mu5u9GrybDdska-ljPXyBjTpdZIUev_6i7t4LRDfXM8/edit#gid=0):

### 2.1 Sanity Check
We expect traffic will have split evenly to control and experiment group , and we use 95% condifence interval,we have calculation as follows:

`Number of cookies:
SE=sqrt(0.5*0.5*(1/344660+1/345543))=0.0006
Magin of error=1.96*0.0006=0.0012
Lower bound~Upper bound=(0.5-0.0012,0.5+0.0012)=(0.4988,0.5012)`

`Number of clicks:
SE=sqrt(0.5*0.5*(1/28325+1/28378))=0.0021
Magin of error=0.0021*1.96=0.0041
Lower bound~Upper bound=(0.5-0.0041,0.5+0.0041)=(0.4959,0.5041)`

Here we have result table:

| Invariant Metrics | Expect Value | Observed Value | CI Range      | Result |
|-------------------|--------------|----------------|---------------|--------|
| Number of cookies | 0.5          | 0.5006         | 0.4988-0.5012 | PASS   |
| Number of clicks  | 0.5          | 0.5005         | 0.4959-0.5042 | PASS   |

### 2.2 Result Analysis

#### 2.2.1 Effect Size Test

| Evaluation Metrics | d(min) | Difference between two groups | CI Range        | Result                                                |
|--------------------|--------|-------------------------------|-----------------|-------------------------------------------------------|
| Gross Conversion   | 0.1    | -0.02055                      | -0.0291~-0.0120 | Statistically and Practically significant     |
| Net Conversion     | 0.0075 | -0.00487                       | -0.0116~0.0019  | Statistically and Practically insignificant |

### 2.2.2 Sign Tests

Use sign and binomial test [calculatetor](http://graphpad.com/quickcalcs/binomial1.cfm):

Gross Conversion:

`Number of days which experiment gross conversion >control gross conversion: 4`

 `Number of days of testing: 23`
 
 `Two-tail p-value: 0.0026`
 
 P-value=0.0026 <alpha=0.025 ,so gross conversion is statistically significant.
 
 Net Conversion:
 
 `Number of days which experiment gross conversion > control net conversion: 10`
 
  `Number of days of testing:23`
  
  `Two-tail p-value: 0.6776`
  
 P-value=0.6776 >alpha=0.025, so net conversion is not statistically significant.
 
 ## 3.Summary
 
 From effect size tests we can see gross conversion is both statistically and practically significant, while net conversion is neither.So we does not support the hypothesis, and think change screener won't help inproving students whole experience.  
 
