This codebook describes the code run_analysis.R
Purpose:  To clean up a dataset on variables collected from a wearable computing technology – specifically the accelerometers from the Samsung Galaxy S smartphone.  This tidy version of the data can be used for later analysis.  
Raw Data: The data and information on the methods for the Samsung Galaxy S smartphone accelerometers can be obtained from the site below.   
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
Citation:
Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. A Public Domain Dataset for Human Activity Recognition Using Smartphones. 21th European Symposium on Artificial Neural Networks, Computational Intelligence and Machine Learning, ESANN 2013. Bruges, Belgium 24-26 April 2013.

Codebook:  The code reads the features.txt, x_train.txt, and x_test.txt datasets.  Features were used to label columns for the x_train/test datasets.  The x_test/train datasets were merged into one file using a rowbind function. The merged dataset was simplified to only the columns that contained the mean or standard deviation.  The numeric codes for activity level y_train.txt and Y_test.txt were read in and merged together.  The result was added as a column to the variable dataset.  A ifelse chain was to assign the Activity Level – WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING or LAYING.  Column names were reassigned to be read clearly by the user.  The data was summarized to calculate the variable mean for each Activity Level for each subject in the study – XYZ.  The data was formatted to follow the tidy data standard whereby the subject (XYZ) and the measured variables (BodyAcc, GravityAcc-, BodyAccJerk, BodyGyro, BodyGyroJerk, fBodyAcc, fGravityAcc-, fBodyAccJerk, fBodyGyro,f BodyGyroJerk) are in separate columns.   
"Activity.Level" – when the measurements were taken
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING
 "measurement_mean" – mean of the measurements collected from cell data

BodyAcc.mean
GravityAcc.mean
BodyAccJerk.mean
BodyGyro.mean
BodyGyroJerk.mean
fBodyAcc.mean
fBodyAccJerk.mean
fBodyGyro.mean

"subject"  - unique subject
X
Y
Z
"mean" – means of the means for each measurement variable by activity level for each variable - gravity units 'g'
“measurement_std” – standard deviation of the measurements collected from cell data
BodyAcc.std
GravityAcc.std
BodyAccJerk.std
BodyGyro.std
BodyGyroJerk.std
fBodyAcc.std
fBodyAccJerk.std
fBodyGyro.std
"standard.deviation" – mean of the standard deviation for each measurement variable by activity level
