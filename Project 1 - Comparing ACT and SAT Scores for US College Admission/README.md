# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 1: Standardized Test Analysis

## Problem Statement/ Background

This project is meant to help high school students decide between taking ACT or SAT for their college admission.

SAT and ACT are standardized tests that many colleges and universities in the United States require for their admission process. Since the advent of ACT, the SAT and ACT have been in [rivalry](https://www.bestcolleges.com/blog/history-of-act/). Despite their [differences](https://www.crimsoneducation.org/sg/blog/test-prep/sat-vs-act-whats-the-difference/), there have been online resources to convert SAT and ACT interchangably. For example, see [the princeton review](https://www.princetonreview.com/college-advice/act-to-sat-conversion), [crimson education](https://www.crimsoneducation.org/sg/blog/test-prep/sat-vs-act-whats-the-difference/) etc.

This project shall aim to:
1. examine the reliability of the SAT and ACT concordance table taken from the respective official board website by comparing it with the college admission scores and stats admission scores; 
2. advise students on which test to take in order to better their chance to enroll in their dream colleges;
3. advise students on which test to fare better given the state they reside in; and
4. provide further analysis using logistic regression on the findings from point 2 and point 3.

## Datasets

The project will make use of the following datasets for analysis:
1. [`act_2019.csv`](./data/act_2019.csv): 2019 ACT Scores by State ([*source*](https://blog.prepscholar.com/act-scores-by-state-averages-highs-and-lows))
2. [`sat_2019.csv`](./data/sat_2019.csv): 2019 SAT Scores by State ([*source*](https://blog.prepscholar.com/average-sat-scores-by-state-most-recent))
3. [`sat_act_by_college.csv`](./data/sat_act_by_college.csv): Ranges of Accepted ACT & SAT Student Scores by Colleges ([*source*](https://www.compassprep.com/college-profiles/))
4. [`sat_act_score_convertor.csv`](./data/sat_act_score_convertor.csv): ACT & SAT Student Scores Concordance Table from offical websites (sources: [ACT](https://www.act.org/content/act/en/products-and-services/the-act/scores/act-sat-concordance.html) & [SAT](https://satsuite.collegeboard.org/higher-ed-professionals/score-reports/score-comparisons/sat-act))

## Key Findings 

The following are the key findings of this project:

1. The SAT and ACT concordance table taken from the respective official SAT and ACT website is useful as a convertor for SAT and ACT scores in general.
2. However, as we propped deeper into individual colleges and states, there is a preference for either tests even though the most colleges and states are neutral. 
3. There is an obvious preference for ACT score for colleges. Apart from the only 15 colleges that prefers SAT scores, high school students are advised to take ACT to increase their likelihood of getting into college.
4. Slightly more states do better in SAT test than ACT test. High school students are also suggested to base their decision to take either test based on their geographical location.
5. Logistic regression analysis from the college dataset does not return any promising result.
6. Logistic regression analysis from the state dataset reveals that the participation rate of ACT is a fairly good predictor on the best performing test for each state.