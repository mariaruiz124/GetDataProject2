# GetDataProject2
Repository for Getting and Cleaning Data project
# Script run_analysis.R
library(plyr) # for function join
        
# Read training and test sets          
TrainSet <- read.table("./GetdataProject/UCI HAR Dataset/train/X_train.txt", header = FALSE, sep = "")
TestSet <- read.table("./GetdataProject/UCI HAR Dataset/test/X_test.txt", header = FALSE, sep = "")

# Read variable names from file features.txt
FeaturesData <- read.table("./GetdataProject/UCI HAR Dataset/features.txt", header = FALSE, sep = "")

# Name the columns in sets data frames with the names from features.txt                           
colnames(TrainSet) <- FeaturesData[,2]
colnames(TestSet) <- FeaturesData[,2]

# Read subject data from train and test
SubjectTrain <- read.table("./GetdataProject/UCI HAR Dataset/train/subject_train.txt", header = FALSE, sep = "")
SubjectTest <- read.table("./GetdataProject/UCI HAR Dataset/test/subject_test.txt", header = FALSE, sep = "")

colnames(SubjectTrain) <- "subject"
colnames(SubjectTest) <- "subject"

# Read activities labels from train and test
LabelsTrain <- read.table("./GetdataProject/UCI HAR Dataset/train/Y_train.txt", header = FALSE, sep = "")
LabelsTest <- read.table("./GetdataProject/UCI HAR Dataset/test/Y_test.txt", header = FALSE, sep = "")

# Read the descriptive activity names for the activities numeric codes
LabelsText<- read.table("./GetdataProject/UCI HAR Dataset/activity_labels.txt", header = FALSE, sep = "")

# Join codes and names
LabelsTrain <- join(LabelsTrain, LabelsText)
LabelsTest <- join(LabelsTest, LabelsText)

# name columns in the activities data frames
colnames(LabelsTrain) <- c("code","activity")
colnames(LabelsTest) <- c("code","activity")

# Join the colums from the three tables (subject, activity and data sets)
TrainData <- cbind("set" = "train", SubjectTrain, "activity" = LabelsTrain$activity, TrainSet)
TestData <- cbind("set" = "test", SubjectTest, "activity" = LabelsTest$activity, TestSet)

# Merge both data sets
dataset <- rbind (TrainData, TestData)

#### Extracts only the measurements on the mean and standard deviation for each measurement. 

columnsStd <- which(grepl("std", names(dataset)))
columnsMean <- which(grepl("mean", names(dataset)))

# Only columns with the strings "std" or "mean" in their names are selected, plus the 
# "activity" column and the "subject" column. The column "set" (column 1) is not needed
dataset <- dataset[,c(2:3, columnsMean, columnsStd)]

# Generate the data frame "tidydata" that contains the mean values for each variable 
# grouped by activity and subject
tidydata <- dataset%>%
        group_by(activity,subject)%>% 
        summarise_each(funs(mean), 3:81)

# To create a text file with the data from tidydata
# write.table(tidydata, "./GetdataProject/tidydata.txt", row.name=FALSE)
