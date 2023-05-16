# MACHINE LEARNING PROJECT: ASTEROIDS CLASSIFICATION
### Group 11: Alessandro Scicluna, Ignazio Leonardo Scarpelli, Lorenzo Carucci

## Introduction
In this project, the most famous space program agency in the world gave us the very important task of gaining insights on the characteristics of some NEO (Near Earth Objects) which can potentially hit the Earth. To do so, the agency provided us with a dataset which includes more than four thousand different NEOs observed between 1995 and 2016 together with their respective data. Our goal is to build a model that can predict, following some patterns and trends in the data, if an asteroyd could be or not dangerous for ourself.  

## Methods
After having visualized the dataset, we proceeded on cleaning the dataset. Specifically, we threw away some columns which were not useful for our task (such as some "EDT Dia" in different measure units) by creating a new dataset with onnly the useful columns. After that, we checked for missing values and duplicates, and it seemed the dataset didn't have them. However, after checking the 'Neo Reference ID', a unique code to identify the, it turned out that there actually were 751 instances with the same ID. 
