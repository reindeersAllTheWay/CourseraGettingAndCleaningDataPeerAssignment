Code Book
The run_analysis.R script performs the data preparation and then followed by the 5 steps required as described in the course project’s definition.

1. Download the dataset
Dataset downloaded and extracted under the folder called UCI HAR Dataset

Note: If trouble in downloading the zip file, replace http instead of https
> if (!file.exists(filename)){
+   fileURL <- "http://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
+   download.file(fileURL, filename, method="curl")
+ }  
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 59.6M  100 59.6M    0     0   323k      0  0:03:08  0:03:08 --:--:--  318k 
2. Assign all data frames
features <- features.txt : `561 rows, 2 columns' 
The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ.
             features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
activities <- activity_labels.txt : 6 rows, 2 columns 

List of activities performed when the corresponding measurements were taken and its codes (labels)
             activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
subject_test <- test/subject_test.txt : 2947 rows, 1 column 

Contains test data of 9/30 volunteer test subjects being observed
             subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
x_test <- test/X_test.txt : 2947 rows, 561 columns 

Contains recorded features test data
              x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
y_test <- test/y_test.txt : 2947 rows, 1 columns 

Contains test data of activities’code labels
              y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
subject_train <- test/subject_train.txt : 7352 rows, 1 column 

Contains train data of 21/30 volunteer subjects being observed
              subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
x_train <- test/X_train.txt : 7352 rows, 561 columns 

Contains recorded features train data
              x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
y_train <- test/y_train.txt : 7352 rows, 1 columns 

Contains train data of activities’code labels
              y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")
3. Merges the training and the test sets to create one data set
X (10299 rows, 561 columns) is created by merging x_train and x_test using rbind() function
                                     X <- rbind(x_train, x_test)
Y (10299 rows, 1 column) is created by merging y_train and y_test using rbind() function
                                      Y <- rbind(y_train, y_test)
Subject (10299 rows, 1 column) is created by merging subject_train and subject_test using rbind() function
                             Subject <- rbind(subject_train, subject_test)
Merged_Data (10299 rows, 563 column) is created by merging Subject, Y and X using cbind() function
                                  Merged_Data <- cbind(Subject, Y, X)
4. Extracts only the measurements on the mean and standard deviation for each measurement
TidyData (10299 rows, 88 columns) is created by subsetting Merged_Data, selecting only columns: subject, code and the measurements on the mean and standard deviation (std) for each measurement

              TidyData <- Merged_Data %>% select(subject, code, contains("mean"), contains("std"))
5. Uses descriptive activity names to name the activities in the data set
Entire numbers in code column of the TidyData replaced with corresponding activity taken from second column of the activities variable

TidyData$code <- activities[TidyData$code, 2]
6. Appropriately labels the data set with descriptive variable names
code column in TidyData renamed into activities
All Acc in column’s name replaced by Accelerometer
All Gyro in column’s name replaced by Gyroscope
All BodyBod in column’s name replaced by Body
All Mag in column’s name replaced by Magnitude
All start with character f in column’s name replaced by Frequency
All start with character t in column’s name replaced by Time
        names(TidyData)[2] = "activity"
        names(TidyData)<-gsub("Acc", "Accelerometer", names(TidyData))
        names(TidyData)<-gsub("Gyro", "Gyroscope", names(TidyData))
        names(TidyData)<-gsub("BodyBody", "Body", names(TidyData))
        names(TidyData)<-gsub("Mag", "Magnitude", names(TidyData))
        names(TidyData)<-gsub("^t", "Time", names(TidyData))
        names(TidyData)<-gsub("^f", "Frequency", names(TidyData))
        names(TidyData)<-gsub("tBody", "TimeBody", names(TidyData))
        names(TidyData)<-gsub("-mean()", "Mean", names(TidyData), ignore.case = TRUE)
        names(TidyData)<-gsub("-std()", "STD", names(TidyData), ignore.case = TRUE)
        names(TidyData)<-gsub("-freq()", "Frequency", names(TidyData), ignore.case = TRUE)
        names(TidyData)<-gsub("angle", "Angle", names(TidyData))
        names(TidyData)<-gsub("gravity", "Gravity", names(TidyData))
7. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
FinalData (180 rows, 88 columns) is created by sumarizing TidyData taking the means of each variable for each activity and each subject, after groupped by subject and activity.
Export FinalData into FinalData.txt file.
FinalData <- TidyData %>%
  group_by(subject, activity) %>%
  summarise_all(funs(mean))
write.table(FinalData, "FinalData.txt", row.name=FALSE)

str(FinalData)
