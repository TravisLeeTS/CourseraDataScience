# CourseraDataScience

## STEP 0: load required packages
# load the reshape2 package (will be used in STEP 5)

## STEP 1: Merges the training and the test sets to create one data set

# read data into data frames
set directory to test file and read subject_test, X_test and Y_test
set directory to test file and read subject_train, X_train and Y_train

# add column name for subject files (feature)

# add column names for measurement files (X)
set directory to feature file and add column names from feature file to X_train and X_test

# add column name for label files (Y)

# combine feature,X,Y files into one dataset

## STEP 2: Extracts only the measurements on the mean and standard
## deviation for each measurement.

# determine which columns contain "mean()" or "std()"
meanstdcols <- grepl("mean\\(\\)", names(combined)) |
        grepl("std\\(\\)", names(combined))

# ensure that we also keep the subjectID and activity columns
meanstdcols[1:2] <- TRUE

# remove unnecessary columns
combined <- combined[, meanstdcols]

## STEP 3: Uses descriptive activity names to name the activities
## in the data set.
## STEP 4: Appropriately labels the data set with descriptive
## activity names. 

# convert the activity column from integer to factor
combined$activity <- factor(combined$activity, labels=c("Walking",
                                                        "Walking Upstairs", "Walking Downstairs", "Sitting", "Standing", "Laying"))


## STEP 5: Creates a second, independent tidy data set with the
## average of each variable for each activity and each subject.

# create the tidy data set
melted <- melt(combined, id=c("subjectID","activity"))
tidy <- dcast(melted, subjectID+activity ~ variable, mean)

# write the tidy data set to a file
write.csv(tidy, "tidy.csv", row.names=FALSE)
