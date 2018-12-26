# Peer Review: Getting and Cleaning Data project
This repository is my submission for the JHU Getting and Cleaning Data course project. 
This is an exercise on how to get and clean data.

## Dataset
The dataset is the Human Activity Recognition database built from the recordings of 30 subjects performing activities of daily living (ADL) while carrying a waist-mounted smartphone with embedded inertial sensors.This data is from the UCI Machine Learning repo

[Human Activity Recognition Using Smartphones](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones)

## Files
1. `CodeBook.md` a code book that describes the variables, the data, and transformations

2. `run_analysis.R` is an R script that executes the following:
    - Downloading and getting the data
    - Merging the training and test sets to create one data set.
    - Extracting only the measurements on the mean and standard deviation for each measurement.
    - Use of descriptive activity names to name the activities in the data set
    - Label the data set with descriptive variable names.
    -  From the labelled data set, create a second,independent tidy data set with the average of each variable for each activity and each subject.
3. `FinalTidyData.txt ` is the final tidy data. 
