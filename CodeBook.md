# Code book 

The script run_analysis.R will tidy up the data collected in "Human Activity Recognition Using Smartphones Data Set" found in 
"http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones".

The training data has been stored in the data frame TrainSet and the data from the test set has been stored in the TestSet data frame.

The features in file features.txt will be stored in the data frame FeaturesData.

The different labels (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) will be stored in data frames LabelsText.

The data frames SubjectTrain and SubjectTest have only one variable that is an identifier of the volunteer in the experiment with one entry for each of the tests performed.
Each observation correspond with an observation in one of the data frames TrainSet or TestSet.

We'll add the colums to identify the subject (SubjectTrain and SubjectTest) to the initial data set (TrainSet and TestSet) so we can identify who has done every test.
Then the labels to identify the activity will be added to all the observations (LabelsTrain and LabelsTest).

Finally both test and training data will be joined to form one data frame called "dataset".

From "dataset" the mean only columns with the strings "std" or "mean" in their names are selected, plus the "activity" column and the "subject" column. 

Generate the data frame "tidydata" that contains the mean values for each variable grouped by activity and subject

