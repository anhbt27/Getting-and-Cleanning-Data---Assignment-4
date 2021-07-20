library(dplyr)
library(reshape2)
# Reading data
# Reading train data
subjectTrain <- read.table("./UCI HAR Dataset/train/subject_train.txt", header = F, encoding = 'UTF-8')
xTrain <- read.table("./UCI HAR Dataset/train/X_train.txt", header = F, encoding = 'UTF-8')
yTrain <- read.table("./UCI HAR Dataset/train/y_train.txt", header = F, encoding = 'UTF-8')

# Reading test data
subjectTest <- read.table("./UCI HAR Dataset/test/subject_test.txt", header = F, encoding = 'UTF-8')
xTest <- read.table("./UCI HAR Dataset/test/X_test.txt", header = F, encoding = 'UTF-8')
yTest <- read.table("./UCI HAR Dataset/test/y_test.txt", header = F, encoding = 'UTF-8')

# Reading the feature labels
features <- read.table("./UCI HAR Dataset/features.txt", header = F, encoding = 'UTF-8')

# Reading activity labels data
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt", header = F, encoding = 'UTF-8')

# Modifying name of column in subject and acitivity database
subjectTrain <- rename(subjectTrain, subject = V1)
subjectTest <- rename(subjectTest, subject = V1)

yTrain <- rename(yTrain, activity = V1)
yTest <- rename(yTest, activity = V1)

# Setting column name for data base
names(xTrain) <- features$V2 
names(xTest) <- features$V2

# Merge data together
trainData <- cbind(subjectTrain,xTrain,yTrain)
testData <- cbind(subjectTest,xTest,yTest)

data <- rbind(trainData,testData)

# Extracting only mean and std columns
# Getting only mean and std columns
dataMean_STD <- data[,grep("mean\\(\\)|std\\(\\)",names(data))]

# Merge sub-data with activity and subject database
data <- cbind(data$subject,data$activity,dataMean_STD)
data <- rename(data, subject = 1)
data <- rename(data, activity = 2)

# Replace activity ids by names
data$activity <- factor(data$activity,
                        levels = activity_labels$V1,
                        labels = activity_labels$V2)

# Create tidy data set with the average of each variable for each activity and each subject.
dataReshape <- melt(data,id=c("subject","activity"))
tidyData <- dcast(dataReshape, subject + activity ~ variable,mean)


# Create results file
write.table(tidyData,"tidyUCI.txt", row.names = FALSE, col.names=TRUE, fileEncoding = "UTF-8")
write.table(tidyData,"tidyUCI.csv", row.names = FALSE, col.names=TRUE,sep = ",")