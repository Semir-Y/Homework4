# Homework
x_train <- read.table(("./train/X_train.txt"), header= FALSE)
y_train <- read.table(("./train/y_train.txt"), header = FALSE)
sub_train <- read.table(("./train/subject_train.txt"), header = FALSE)

x_test <- read.table("./test/x_test.txt")
y_test <- read.table("./test/y_test.txt")
sub_test <- read.table("./test/subject_test.txt")

variable_names <- read.table("./features.txt")

activity_labels <- read.table("./activity_labels.txt")

##1 Merge training and test sets to create one data

X_total <- rbind(x_train, x_test)
Y_total <- rbind(y_train, y_test)

Sub_total <- rbind(sub_train, sub_test)


#2 Extracts only the measurement mean sd for measurement

selected_var <- variable_names[grep("mean\\(\\)", variable_names[,2]),]
X_total <- X_total[,selected_var[,1]]

#3 Descriptive activity

colnames(Y_total)<- "activity"
Y_total$activitylabel <- factor(Y_total$activity, labels = as.character(activity_labels[,2]))
activitylabel <- Y_total[,-1]

#4 appropriately labels the data set with descriptive names
colnames(X_total) <- variable_names[selected_var[,1],2]

#5 Creates a second independent data
colnames(Sub_total) <- "subject"
total <- cbind(X_total, activitylabel, Sub_total)
total_mean <- total %>% group_by(activitylabel, subject) %>% summarize_each(funs(mean))
write.table(total_mean, file = "./tidydata.txt", row.names = FALSE, col.names = TRUE)
