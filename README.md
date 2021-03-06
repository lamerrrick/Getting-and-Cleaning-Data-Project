L.Merrick code for course project 

setwd("~/Coursera/G_C Data") # working directory 
library(dplyr)  # package dplyr
library(tidyr)  # package tidyr

### 1 uses the read.table function to bring in data and column labels. #### 
### Merges the training and the test sets to create one data set with rbind.####

## bring in the column names and make a 
f <- read.table("~/Coursera/G_C Data/UCI HAR Dataset/features.txt")
names <- f[,2]

## Bring in the training dataset using the features as column names
train <- read.table("~/Coursera/G_C Data/UCI HAR Dataset/train/X_train.txt", col.names = names)

# bring in test dataset using the features as column names
test <- read.table("~/Coursera/G_C Data/UCI HAR Dataset/test/X_test.txt", col.names = names)

# combine the train and test by rows
both <- rbind(train, test)

### 2. Extracts only the measurements on the mean and standard deviation for each ###
### measurement using the dpylr package functions select and contains. Individual ### selections merged with cbind.####

# subset data sets to just get mean and standard deviation 
tt.mean <- select(both, contains("mean.."))
tt.std <- select(both, contains("std"))
tt.sub <- cbind(tt.mean, tt.std)

### 3. Add activity names to name the activities in the data set. Read in the _Y.txt ###
### datasets which contained numeric codes for the measurment data (_X.tst).  Rbind merges ###
### test and train.  Cbind adds numeric activity code to measurement.  Ifesle chain assigns ###
### add names to the activity codes. Cbind merges.  ###

AL <- read.table("~/Coursera/G_C Data/UCI HAR Dataset/train/Y_train.txt")
AL_test <- read.table("~/Coursera/G_C Data/UCI HAR Dataset/test/Y_test.txt")
AL_both <- rbind(AL, AL_test)
withAL <- cbind(tt.sub, AL_both)
withAL <- rename(withAL, ActivityL_Code = V1)

## add the names to the Activity Level 
Activity.Level  <- ifelse(withAL$ActivityL_Code == 1, "WALKING",
               ifelse(withAL$ActivityL_Code == 2, "WALKING_UPSTAIRS",
                      ifelse(withAL$ActivityL_Code == 3, "WALKING_DOWNSTAIRS",
                             ifelse(withAL$ActivityL_Code == 4, "SITTING",
                                 ifelse(withAL$ActivityL_Code == 5, "STANDING",  
                                    ifelse(withAL$ActivityL_Code == 6, "LAYING", -999))))))
withAL2 <- cbind(withAL, Activity.Level)

### 4. Use Colnames to labels the data set with descriptive variable names. ###

#rename column head with "clean names" - I used find and replace - Certainly there is a better way
colnames(withAL2) <- c("BodyAcc.mean_X","BodyAcc.mean_Y", "BodyAcc.mean_Z","GravityAcc.mean_X",                
"GravityAcc.mean_Y","GravityAcc.mean_Z","BodyAccJerk.mean_X","BodyAccJerk.mean_Y",              
"BodyAccJerk.mean_Z","BodyGyro.mean_X","BodyGyro.mean_Y","BodyGyro.mean_Z",                  
"BodyGyroJerk.mean_X","BodyGyroJerk.mean_Y", "BodyGyroJerk.mean_Z","BodyAccMag.mean",                  
"GravityAccMag.mean","BodyAccJerkMag.mean","BodyGyroMag.mean","BodyGyroJerkMag.mean",             
"fBodyAcc.mean_X","fBodyAcc.mean_Y","fBodyAcc.mean_Z","fBodyAccJerk.mean_X",              
"fBodyAccJerk.mean_Y","fBodyAccJerk.mean_Z","fBodyGyro.mean_X","fBodyGyro.mean_Y",                  
"fBodyGyro.mean_Z","fBodyAccMag.mean","fBodyAccJerkMag.mean","fBodyGyroMag.mean",             
"fBodyGyroJerkMag.mean","angle.BodyAccJerkMeangravityMean.","BodyAcc.std_X","BodyAcc.std_Y",
"BodyAcc.std_Z","GravityAcc.std_X","GravityAcc.std_Y","GravityAcc.std_Z",                
"BodyAccJerk.std_X","BodyAccJerk.std_Y" ,"BodyAccJerk.std_Z","BodyGyro.std_X",                  
"BodyGyro.std_Y","BodyGyro.std_Z","BodyGyroJerk.std_X","BodyGyroJerk.std_Y",               
"BodyGyroJerk.std_Z","BodyAccMag.std" , "tGravityAccMag.std","BodyAccJerkMag.std",               
"BodyGyroMag.std","BodyGyroJerkMag.std", "fBodyAcc.std_X","fBodyAcc.std_Y",
"fBodyAcc.std_Z","fBodyAccJerk.std_X","fBodyAccJerk.std_Y","fBodyAccJerk.std_Z",                
"fBodyGyro.std_X","fBodyGyro.std_Y","fBodyGyro.std_Z","fBodyAccMag.std" ,                  
"fBodyAccJerkMag.std","fBodyGyroMag.std","fBodyGyroJerkMag.std","ActivityL_Code",  
"Activity.Level") 

### 5. Follow Tidy data principles to clean the dataset. Dplyr package group_by and summarize to get ###
### means for each activity level for each measurement. Used the dpylr and tidyr package functions ###
### select, gather and separate to cut rearrange the data to get all subjects in one column and the ###
### mean and standard deviation measurement categories and results into individual columns. ###

# Summarizr the mean for each variable 
Al_mean <- group_by(withAL2, Activity.Level)
Al_mean2 <- summarise_each(Al_mean, funs(mean), BodyAcc.mean_X:fBodyGyroJerkMag.std)
Al_mean_sub <- select(Al_mean2, Activity.Level, contains("_"))

# cleans the data to follow tidy data rules.  
means <- select(Al_mean_sub, Activity.Level,contains("mean"))
t <- gather(means, measurement_subject , mean, -Activity.Level)
t2 <- separate(t, col= measurement_subject, c("measurement_mean", "subject"), sep = "_")

std <- select(Al_mean_sub, Activity.Level,contains("std"))
s <- gather(std, measurement_subject , standard.deviation, -Activity.Level)
s2 <- separate(s, col= measurement_subject, c("measurement_std", "subject"), sep = "_")
s4 <- s2[,c(2,4)]

## merge the standard deviation to the tidy data set. 
Final <- cbind(t2,s4)

# Write as a table
write.table(Final, "TidyData.txt", row.name=FALSE)