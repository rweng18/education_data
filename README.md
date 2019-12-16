# Classifying Degree-Granting Postsecondary Institutions by Census Bureau Region
This project aims to explore whether a machine learning algorithm can accurately classify a degree-granting postsecondary institution as being located in the Northeast or the West, as defined by the Census Bureau.

## Data
The data from this project was obtained through the [Education Data Portal API](https://educationdata.urban.org/documentation/colleges.html#scorecard-student-characteristics-student-aid-applicant-characteristics) via the Urban Institute. The goal of Urban Institute's project was to make education data easy to access and analyze for greater transparency and hopefully positive policy changes.

My project focused on the data available for postsecondary institutions only. I further subset the data for institutions that are degree-granting and located in the Northeast and the West. The final dataset contains 1,129 institutions and 27 predictor variables containing information about those institutions and their student bodies in 2016.

* Integrated Postsecondary Educaton Data System (IPEDS)
     + Institional Characteristics
     + Headcount (Enrolled student characteristics)
* College Scorecard
     + Student Home and Neighborhood Characteristics (for financial aid applicants)
* Dependent Variable of Interest
     + Census Bureau Region

### Feature Selection
The data pulled from the IPEDS and Scorecard data originally started with 100+ variables, that provided tons of information about each institution and its student population. Based on domain knowledge of what may be most relevant to the distinctions between schools, I focused on the racial breakdown of enrolled students, institutional characteristics (size, on-campus housing, years of education offered, etc.), and socioeconomic breakdown of financial aid applicants.

### Outcome Variable
<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig01_cb_region_hist.jpg" width = "400" height = "300">

## Models
### Evaluation Metrics
The metric I focused on was accuracy, given that the two classes were balanced. It did not matter from a practical standpoint whether the model was better at classifying institutions in the Northeast or the West. However, I did look at precision, recall, and F1 score for all models, for both the training and testing datasets.

### Baseline
For the baseline model, I used the Dummy Classifier provided by Scikit Learn, using the "stratified" method, which relies only upon the proportion of data that is classified in each group. Unsurprisingly, the dummy classifier thus yielded an accuracy of only 45%.

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig23_cm_dummy.jpg" width = "400" height = "275">

### Models Run
* K-Nearest Neighbors
     * Scaled variables
     * Evaluate for k = 1-25
     * Optimal: k = 3
* Baseline Decision Tree
     * Evaluate for maximum depth = 1-10
     * Optimal: max_depth = 8
* GridSearch Optimized Decision Tree
     * criterion = entropy
     * max_depth = 10
     * min_samples_split = 2
* Baseline Random Forest
     * n_estimators = 100
     * max_depth = 2
* GridSearch Optimized Random Forest
     * criterion = entropy
     * max_depth = 10
     * max_features = auto (sqrt(number of features))
     * min_sample_split = 5
     * n_estimators = 125
* AdaBoost
     * Baseline: n_estimators = 100
     * Optimal: n_estimators = 175
* GridSearch Optimized XGBoost
     * learning_rate = 0.1
     * max_depth = 6
     * min_child_weight = 1
     * n_estimators = 100
     * subsample = 0.5

### Final Model: GridSearch Optimized Random Forest

<img src = "https://github.com/rweng18/education_data/blob/master/tables/comparing_final_models.jpeg" width = "400" height = "175">

The above table compares evaluation metrics for the baseline model, the random forest model, and the optimized random forest model, for both training and testing data. From the table, we can see that the optimized random forest model achieves a 93% accuracy rate on the testing data, while the baseline random forest only has an 82% accuracy rate. However, the optimized random forest may be overfitting slightly on the training data, but based on the evaluation metrics and interpretability, this was the best model at my disposal.

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig26_compare_final_models_roc.jpg" width = "400" height = "300">

Comparing the ROC curves for the baseline, random forest, and optimized random forest reinforce the idea that the optimized random forest is best at balancing the false positive rate with the true positive rate at all classification thresholds.

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig25_cm_forest_opt.jpg" width = "400" height = "275">

The confusion matrix for the optimized random forest shows that the final model correctly classifies most of the testing data. It may be slightly better at classifying schools in the Northeast than schools in the West.

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig27_forest_opt_feat_impt.jpg" width = "900" height = "550">

In the above plot, the red dotted line differentiates between characteristics of the student population and characteristics of the institution itself. The most important characteristics appear to be race proportions, followed by financial information about aid applicants, and then institutional characteristics.

## Conclusion
Given how significant the racial breakdown of the enrolled studnet population was in terms of classifying colleges, there are implications about how different the underlying populations are. As a result, this may reflect upon student experiences at these institutions. These results reinforce the idea that diversity at instititutions is only perceived, and has implications about the realities of social mobility.

### Limitations
In this iteration of the project, due to time constraints, columns that were missing 10% or more of their data were dropped. With the remaining variables, all rows were dropped with any missing data. Future work should make attempts to impute the missing data, as well as provide exploratory data analysis to see if there are patterns in terms of which institutions are missing data.

### Future Work
The work is hardly done in terms of understanding how student postsecondary experiences may vary across the US. Future projects include multiclass classification and analysis of the observations that this model incorrectly classified. It would also be worthwhile to see if data could be collected that could inform people about graduation rates based on socioeconomic status and race, dependent on the type of institution they attend.
