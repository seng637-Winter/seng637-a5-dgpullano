**SENG 637- Dependability and Reliability of Software Systems***

**Lab. Report \#5 – Software Reliability Assessment**

| Group \#:       |   |
|-----------------|---|
| Student Names:  |   |
|                 |   |
|                 |   |
|                 |   |

# Introduction


#

# Assessment Using Reliability Growth Testing 

#

## Result of Model Comparison (Selecting Top Two Models)
For the first part of this report, we decided to use C-SFRAT to visualize the hypothetical SUT from the failure data provided to us. The windows executable for this was downloaded from the provided GitHub link. 
The document found at https://www.sciencedirect.com/science/article/pii/S2352711021001588 was read to gain a better understanding of how to use the C-SFRAT application and interpret its output results.

Initially, we imported the provided failure data and ran estimations using all combinations of hazard functions and covariate sets. 
This was done, as we wanted to compare all possible results to explore which models worked well and which did not. 

We decided to use the Akaike Information Criterion (AIC), Bayesian Information Criteria (BIC), and sum of squares (SSE) as measures for goodness-of-fit to evaluate the different hazard function models that use various covariate combinations as inputs. 

The AIC uses the log-likelihood measure as its initial estimate [https://builtin.com/data-science/what-is-aic], but then applies a penalty term to account for the complexity of the model (i.e. how many parameters the model uses).
The BIC is calculated in a similar process to the AIC, where it starts with the log-likeliness value and applies a penalty term based on the number of model parameters used [https://medium.com/@analyttica/what-is-bayesian-information-criterion-bic-b3396a894be6]. 
The difference between the two is that the BIC introduces a penalty term that is larger than the one applied in the AIC calculation.

The reason we decided to use the AIC and BIC instead of the log-likelihood metric, was that the models being compared against each other in this assignment have a different number of covariates that are included in them. 
We believe that it is not good practice to use log-likeliness for all models in this assignment because the ones with a larger number of covariates will likely produce more model parameters and an artificially higher log-likelihood value.
A model that produces a lower AIC or BIC value fits the data better than other models it is being compared against that have higher AIC or BIC values.

The SSE is the last metric that we decided to use, since it is a simple measure that is calculated by summing the squared differences between the actual failure data and the model predicted data [https://prepnuggets.com/glossary/sum-of-squared-errors/#:~:text=It%20is%20calculated%20by%20summing,indicates%20a%20poorer%20fitting%20model.]. 
Similar to the AIC and BIC, a lower SSE value is desired when comparing models against each other.

With these metrics under consideration, we had narrowed the best models down to 3 possibilities that can be seen in the table below:

![Table 1: Initial 3 Best Potential Models](Submission_Screenshots/RGT_Initial3Best.png)

For all metrics considered, model 32 that used the DW3 (Discrete Weibull-Type III) hazard function with the F (Failure identification work measured in person hours) covariate produced the best results of 122.199, 127.935, and 528.046 respectively.
The other 2 models considered for second place were as follows:
- Model 14 that used the IFRGSB (IFR Generalized Salvia and Bollinger) hazard function with the E (Execution time measured in hours), F, and C(Computer time failure identification measured in hours) covariates. 
- Model 37 that used the GM (Geometric) hazard function with the F covariate.
	
If we made our decision based on the AIC and BIC metrics alone, the second best model would have been model 37, since the AIC and BIC values for model 37 were lower than that of model 14 by 20.702 and 25.004 respectively,
However, the SSE for model 14 was much lower than that of model 37 by 154.338. For this reason, we decided that the second best model performance was attributed to model 14.

Visualizations for MVF and Intensity graphs for these 2 best performing models once they were re-ran through the system without most of the other models are presented below:

![Figure 1: Initial MVF Graph for 2 Best Models](Submission_Screenshots/RGT_Initial_MVF_Graph_2Best.png)

Looking at the initial MVF graph above, it can be seen that the model predictions are generally lower than the actual data points between intervals 2 - 9.
There is then an inflection point which reverses this relationship to that the predictions are generally higher until around iteration 22.

![Figure 2: Initial Intensity Graph for 2 Best Models](Submission_Screenshots/RGT_Initial_Intensity_Graph_2Best.png)

Looking at the initial intensity graph above, it can be seen that even in the best performing models, there are still several areas where the dataset intensities are much higher than those predicted by the 2 models (i.e. data points 2, 19, 20, 21, 22).
There are also several locations where the dataset intensities are much lower than those predicted by the 2 models (i.e. data points 3, 6, 7, 16, 24).
This suggests that the models can be improved through performing a range analysis on these 2 best models.

#

## Result of Range Analysis (Which Part of Data is Good for Proceeding with Analysis)
Now that the 2 best models have been identified, the next step was to perform a range analysis using subsets of the provided dataset to identify which part of the data is good for proceeding forward.

We started testing various ranges using between 19 and 22 data points to run model estimations. 
We chose this as our initial place to start since both models had a very low difference between predicted and the original data at data points 20 and 21. 
Additionally, data points 19 - 21 had the largest changes (increases) in subsequent failures per interval.

We found that by running the 2 models up to data point 20 (where the largest change in failures occurred between 2 data points), and then applying the efforts per interval of (E = 0, F = 24, C = 3) over the remaining 11 intervals, we were able to produce the following metrics:

![Table 2: Range Analysis - One of Best Results](Submission_Screenshots/RGT_MetricsDataPoint20ExtendedTo31_E_0_F_24_C_3.png)

When comparing the metrics in Table 2 above with the initial metrics found in Table 1, it can be seen that the AIC, BIC, and SSE values have decreased.
By running initial estimation of the 2 models up to the first 20 data points (approximataly a 65% subset of the original data), and then predicting the remaining 11 intervals with the efforts per interval of (E = 0, F = 24, C = 3), we were able to make the following improvements:
- IFRGSB (E, F, C):
     - AIC improved by 65.315 from 146.025 to 80.710    --> (44.7% improvement)
     - BIC improved by 67.944 from 154.629 to 86.685    --> (43.9% improvement)
     - SSE improved by 489.954 from 605.317 to 115.363  --> (80.9% improvement)
- DW3 (F):
     - AIC improved by 43.563 from 122.199 to 78.636    --> (35.6% improvement)
     - BIC improved by 45.316 from 127.935 to 82.619    --> (35.4% improvement)
     - SSE improved by 254.445 from 528.046 to 273.601  --> (48.2% improvement)
		
These changes can be seen visually in Figures 3 and 4 below and can be compared against Figures 1 and 2, respectively:

![Figure 3: Range Analysis MVF Graph for 2 Best Models](Submission_Screenshots/RGT_MVF_DataPoint20ExtendedTo31_E_0_F_24_C_3.png)

When comparing the MVF graph above with that of our initial runs, it can be seen that both models are tighter to the various failure intervals after our modifications.
Similar patterns of the estimations being under the actual failures between interval 2 - 9 and over the actual failures between interval 9 - 20 still exist after modifications.

![Figure 4: Range Analysis Intensity Graph for 2 Best Models](Submission_Screenshots/RGT_Intensity_DataPoint20ExtendedTo31_E_0_F_24_C_3.png)

When comparing the intensity graph above with that of our initial runs, there are still several areas where the predicted intensities are lower than the actual failure data (i.e. data points 2, 4, 5, 19, and 20), and there are also still a few areas where the predicted intensities are higher than the actual failure data (i.e. data points 9, 11, and 16).

Overall, it can be seen that the dataset intensities line up allot better to the model estimations after our modifications were made, where the differences are of less intensity.

#

## Discussion on Decision Making (Given A Target Failure Rate)

The first thing to consider when making any decisions related to targetted failure rate while performing reliability growth testing is to determine what the acceptable targetted failure rate is for the type of SUT you are working with.
This could be based on common industry standards for the type of SUT you are working with if they exist, or from user/client expected failure rates.
For a comparison example, there is a different acceptable target failure rate that should be applied when testing an app that shows images of cats vs. a system that manages life support systems on a spacecraft.

It is also important to keep in mind that all SUT scenarios will fail at one point or another, and that expectations should be managed accordingly. 
Having an unnecessarily low target failure rate could result in a larger number of resources being allocated to software development that could be spent more efficiently in other parts of overall product development.
In contrast with this, having a target failure rate that is very high could results in a SUT that doesn't meet user/client expectations, or one that doesnt work entirely.

These types of considerations need to be managed appropriately through different stages of software development so that a high quality product can be produced that meets expectations while not being a drain on resources.

## Discussion on Advantages and Disadvantages of Reliability Growth Testing Analysis

Through the completion of the first half of this assignment, we were able to identify several advantages and disadvantages to applying reliability growth testing to a SUT:

### Advantages:
- A SUT can be evaluated using many types of models. 
- These models can be evaluated using different subsets of covariates, which allows the tester to learn about which covariates have a larger effect on the model
- Metrics such as AIC, BIC, and SSE can be calculated by comparing model estimated values with the original failure data. These can then be interpretted in an intuitive fashion to rank the performances of various models.

### Disadvantages:
- Performing reliability growth testing requires specialized software. We found that the software was not able to run consistently on different types of OS (Mac vs. PC)
- When performing range analyses, it was a very tedious process to find the range at which the failure data should have been truncated at for each model before estimating the remaining data points for validation. Even though we were able to improve the performance of our models by at least 35% across all evaluation metrics, it is likely that there is a better solution that we did not find.
- The performance of the models are dependent on the model used. While this could be seen as an advantage (as new models can be added), it is also a disadvantage in that the right model needs to be identified to match with the SUT that is being tested.

#

# Assessment Using Reliability Demonstration Chart 

# 

We followed the following process to perform RDC testing:

The first step was to pick a metric from our initial failure data and to convert it into a format that was acceptable for the RDC excel sheet that was provided for this assignment.
To do this, we first decided that we would use the failure count column (FC), along with the execution time measured in hours (E) to create a new list of failures that occured at specific concurrent times.

For example, if 1 row in the initial failure data was (FC = 4, E = 0.05), we would write 4 new rows in our new table where the 'Count' column went from 1 - 4, and the 'Runtime Since Failure' column would be 0.05 for all 4 new rows. 
Once this process was applied through all rows of the original failure dataset, we had a total of 92 rows.
A third column was created in this new table called 'Total Time'. It started at 0 and added the subsequent 'Runtime Since Failure' value in the second column to the total time that was in the row above.

A sample screenshot of this table format after the conversion had been applied can be seen below. This screenshot represents the first 2 rows of the original failure data that was provided to us with the values being (FC = 2, E = 0.05) and (FC = 11, E = 1):

![Table 3: Sample of Failure Input Table After Conversion](Submission_Screenshots/RDC_FailureData_Conversion_Sample.png)


# Comparison of Results from Part 1 and Part 2

# Discussion on Similarity and Differences of the Two Techniques

# How the team work/effort was divided and managed

# 

# Difficulties encountered, challenges overcome, and lessons learned

# Comments/feedback on the lab itself
