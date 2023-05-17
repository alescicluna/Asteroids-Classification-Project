# MACHINE LEARNING PROJECT: ASTEROIDS CLASSIFICATION
### Group 11: Alessandro Scicluna, Ignazio Leonardo Scarpelli, Lorenzo Carucci

## Introduction
In this project, the most famous space program agency in the world gave us the very important task of gaining insights on the characteristics of some NEO (Near Earth Objects) which can potentially hit the Earth. To do so, the agency provided us with a dataset which includes more than four thousand different NEOs observed and analysed together with their respective data. Our goal is to build a model that can predict, following some patterns in the data, if an asteroyd could be or not dangerous for ourself.  

## Methods
Before starting the EDA, since we are not space's experts, we took some time to better understand the task's arguments and the dataset's features. As an example, to understand the "Jupiter Tisserand Invariant" variable, and if it would be useful for our purpose, we spent some time surfing the internet to deepen our knowledge on the argument. The same could be said for concepts like Epoch Date and other measure units such as Astronomical Units. Then, to simplify our analysis, we decided to get rid of some redundant features, such as the same distances displayed in different measure units (miles, feet or km/h, for example), using meters as our universal measure unit for NEOs' diameters, km/s for the velocity and km for the distance between NEOs and Earth. We deleted variables, such as Equinox and Orbiting Body, which contained only one value too (J2000 for "Equinox" and Earth for "Orbiting Body"). After this, we performed some basic data cleaning tasks such as looking for and dropping duplicates and null values. Although at first glance we didn't find any of both, after a deeper research we found out that the dataset had 751 duplicates. This is probably because the dataset has been updated several times and some duplicates were probably left in the dataset.
To start the EDA, we looked at summary statistics for our numerical features. This gave us a first impression of the data we were trying to model. For instance, we realized that there was a significant difference among the estimated diameter of the asteroids, as well as concerning their relative velocity and distance from the Earth.
Then, we continued looking at the distribution of each parameter. First thing that came to our attention was the distribution of the Hazardous feature, which was unbalanced (84.4% of not potentially dangerous NEOs against 15.6% of potentially dangerous ones).
Since we wanted to avoid the snooping bias, we split the data in training, validation and test set before proceeding with our data exploration. We included a validation set because we wanted to keep the test set for the very end testing, having the opportunity to check different combinations of parameters for the models.
Then, we looked for linear combinations of parameters through the correlation matrix (which use the Pearson’s correlation coefficient). Since we want to avoid multicollinearity, we got rid of those variables that were highly correlated with each other (70% or more, which is considered a good starting rule of thumb).
We proceeded in plotting the remaining parameters (since we wanted to be sure there were no non-linear relationships among features) and the highly correlated ones (30% or more) to look at the data trends. Here we also tried to understand, scientifically speaking, the patterns among some of the correlations. Let's take the ‘eccentricity’ and ‘relative velocity’ one (49% linear correlation) as an example. It turned out that, when the NEO’s orbit has a high eccentricity, it has a wider range of distances between its perigee (closest approach to Earth) and apogee (farer approach). As a consequence, when at its perigee, the stronger gravitational pull from the Earth accelerates it, resulting in a higher relative velocity.
Furthermore, we also noticed that there is a good level of correlation between the ‘distance of the NEO from the Earth’, and the ‘relative velocity’, which is easily explainable with the words used before. Therefore, we choose to create two testing dataset: one without the highly correlated variables, of which we spoke before, and one which saw the four variables most correlated between themselves (after the ones we deleted before) dropped.
Finally, we looked for outliers in the selected hyperparameters by drawing box plots. There are many outliers in some variables, such as ‘Est Dia in M’, ‘Relative Velocity’, ‘Inclination’. However, since we are dealing with a cautious problem, such analysis may require domain expertise and consultation with relevant experts to be sure that any actions taken are appropriate and justified. Therefore, we decided not to eliminate outliers, but subsequently opted for models robust to them.
Even though tree-based models are robust to outliers, since we have many outliers, we still opted for a ‘Robust Scaler’, which uses the median and interquartile range to scale the data, making it more robust to outliers.
                  
## Experimental Design
We started with the model building. Our first choice was the Logistic Regression algorithm. Since it is a simple model, we used its results as a baseline. We evaluated the performances through the cross-validation technique for both the parameters selection, and it turned out that, for this model, the better results came out with the first testing dataset, the one which only missed the features with a linear correlation higher than 70%.
Then, we tested both dataframes on the Random Forest algorithm, and then plotted the feature importance graph. We noticed that three parameters contribute to almost all overall predictive power of the model. (Specifically, ‘minimum orbit intersection’, ‘Est Dia in M(max)’ and ‘Orbit Uncertainity’). However, in this case, the second dataframe (which has more parameters cutted) performed slightly better.
After that, we performed a Randomized Search for optimization, since we wanted to test for better hyperparameters, and tested the model on the validation and test set.
Finally, we tried a more complex and robust method like ‘Adaptive Boosting, a model which can handle class imbalance but is more sensitive to outliers. Here we performed the same process as for the previous model. The feature importance in this model, as expected, were way more distributed since, as AdaBoost progresses, it focuses more on ‘difficult’ examples and adjusts the weights as consequence.
AdaBoost, which is a more complicated model to interpret and does not deal well with outliers, performed worse than Random Forest. As for the Random Forest algorithm, in this model the dataset that performed better was the second one. Since, as said before, the classes regarding the “Hazardous” feature were imbalanced, we also tried to perform an undersampling and retested the algorithms, to see if the results could improve. As for the metrics, we concentrated our analysis on ROC and recall for a simple reason: our goal is to minimize false negatives, which in this case are NEOs that are falsely not labeled as a potential danger.

## Results
As we will see below, the results of our work are, divided by the models we trained: Logistic Regression, Random Forests and AdaBoost. 

Logistic Regression ROC Curve: 

![LOGISTIC](https://github.com/IgnaLS/Group-11-Machine-Learning-Project/assets/132266002/c3ff7039-b685-4a58-87e3-95f959a377a6)


Random Forest and AdaBoost ROC Curve: 

![LOGISTIC AND ADABOOST](https://github.com/IgnaLS/Group-11-Machine-Learning-Project/assets/132266002/750e7864-78c3-438e-85b6-e6026b83d3bd)

LOGISTIC REGRESSION metrics (through cross-validation):

MEAN accuracy score: 0.8742609381158848, MEAN Precision: 0.7127491187862705, MEAN Recall: 0.32524990744168825, MEAN F1 score: 0.44574552562458003


RANDOM FOREST metrics (through cross-validation):

Precision: [0.9952     0.98245614], Recall: [0.99679487 0.97391304], F1 score: [0.9959968  0.97816594]

Confusion matrix: 

[[622   2]

[  3 112]]

ADABOOST  metrics (through cross-validation):

Precision: [0.99520767 0.99115044], Recall: [0.99839744 0.97391304], F1 score: [0.9968     0.98245614]

Confusion matrix: 

[[623   1]

[  3 112]]

RANDOM FOREST WITH UNDERSAMPLING 

Precision: [1.        0.5450237], Recall: [0.84615385 1.        ], F1 score: [0.91666667 0.70552147], Support: [624 115]

Confusion matrix: 

[[528  96]

[  0 115]]

The best algorithm and the one with the most balanced results was, in general, Random Forest, which had the best results together with the dataset with less hyperparameters. Furthermore, if we move to the undersampled test, we obtained a perfect recall metric but very low precision. This is the perfect model to limitate false negatives, even at the price of having very high false positives.

## Conclusions
From this project, through the features selection and features importance in our Random Forest algorithm, we learned that the features which more than others have a major weight in the labeling of a NEO’s dangerousness are its distance from Earth’s orbit and its diameter. Furthermore, we were surprised by the fact that the NEO’s distance from Earth itself was way less important than the two variables we saw before.
Our main suggestions are, other than constantly updating the data, to continuing monitor these two variables, as well as to invest in research projects with the goal of improving the research on NEOs and their dangerousness and the data collection about them, since better data will help a lot in future analysis on these asteroids. 
