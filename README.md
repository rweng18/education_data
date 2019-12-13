# Classifying Degree-Granting Postsecondary Institutions by Census Bureau Region
This project aims to explore whether a machine learning algorithm can accurately classify a degree-granting postsecondary institution as being located in the Northeast or the West, as defined by the Census Bureau.

## Data
The data from this project was obtained through the Education Data Portal API via the Urban Institute. The goal of Urban Institute's project was to make education data easy to access and analyze for greater transparency and hopefully positive policy changes.

My project focused on the data available for postsecondary institutions only. I further subset the data for institutions that are degree-granting and located in the Northeast and the West. The final dataset contains 1,129 institutions and 27 predictor variables containing information about those institutions and their student bodies in 2016.

* Integrated Postsecondary Educaton Data System (IPEDS)
     + Institional Characteristics
     + Headcount (Enrolled student characteristics)
* College Scorecard
     + Student Home and Neighborhood Characteristics (for financial aid applicants)
* Dependent Variable of Interest
     + Census Bureau Region

### Outcome Variable
<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig01_cb_region_hist.jpg" width = "400" height = "300">

## Models
### Baseline
For the baseline model, I used the Dummy Classifier provided by Scikit Learn, using the "stratified" method, which relies only upon the proportion of data that is classified in each group. Unsurprisingly, the dummy classifier thus yielded an accuracy of only 45%.

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig23_cm_dummy.jpg" width = "400" height = "275">

### Model Selection
* What parameters did I tune? Why did I tune these?
* Why Random Forest over Boosting?

### Final Model: GridSearch Optimized Random Forest

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig24_cm_forest.jpg" width = "400" height = "275">

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig26_compare_final_models_roc.jpg" width = "400" height = "300">

<img src = "https://github.com/rweng18/education_data/blob/master/figures/fig27_forest_opt_feat_impt.jpg" width = "900" height = "550">

## Conclusion
Given how significant the racial breakdown of the enrolled studnet population was in terms of classifying colleges, there are implications about how different the underlying populations are. As a result, this may reflect upon student experiences at these institutions. These results reinforce the idea that diversity at instititutions is only perceived, and has implications about the realities of social mobility.

### Limitations
In this iteration of the project, due to time constraints, columns that were missing 10% or more of their data were dropped. With the remaining variables, all rows were dropped with any missing data. Future work should make attempts to impute the missing data, as well as provide exploratory data analysis to see if there are patterns in terms of which institutions are missing data.

### Future Work
The work is hardly done in terms of understanding how student postsecondary experiences may vary across the US. Future projects include multiclass classification and analysis of the observations that this model incorrectly classified. It would also be worthwhile to see if data could be collected that could inform people about graduation rates based on socioeconomic status and race, dependent on the type of institution they attend.
