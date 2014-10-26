Machine-Learning-Project
========================
Practical Machine Learning - WL Exercise 

Background

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset). 

Objective of the project with the approach

  The objective is to Explore the pmi dataset that has been provided

  Clean the dataset

  Perform Cross Validation by creating train, test and validate datasets

  Create model - I have used caret package for this project and have tried using one of the most popular method among many,    [method = lda]

  Training the train dataset first and then testing the model on the test dataset 

  The model once confirmed on the test dataset created by partitioning the train dataset is then executed on the pmi_testing   datest to validate the model. THus by doing so I have ensured that creoss validation is done.
  
  
I have used following algorithim to train and create a model
  - lda
  - rpart
  - nb
  - rf

Before applying the algorithim -
 - I cleaned the data to ensure that I have relevant predictors in my dataset 
 - Converted the non numeric variable to numeric
   WL$num_window <- as.numeric(WL$num_window)

   WL$total_accel_belt <- as.numeric(WL$total_accel_belt)
  
   WL$total_accel_arm <- as.numeric(WL$total_accel_arm)
  
   WL$total_accel_dumbbell <- as.numeric(WL$total_accel_dumbbell)
  
   WL$total_accel_forearm <- as.numeric(WL$total_accel_forearm
   
 - Partioned my pmi_training dataset into two dataset one for training and one to test and named them trainingCV and testingCV repecctively
 
 intrainCV <- createDataPartition(y=WL$classe, p=0.7, list = FALSE)
 
 trainingCV <- WL[intrainCV,]
 
 testingCV  <- WL[-intrainCV,]
 
 dim(trainingCV); dim(testingCV)
 
 
 
 - Used preProcess=c("center","scale") to ensure that the data is normalised
 #Training model using training dataset

 - Executed various algo and found that the method = "lda" gives better result and has lesser performance issues as well
 - Executed the trained model on the training CV
 trainmodlda<-train(classe~.,data = trainingCV,preProcess=c("center","scale"), method = "lda")
 
      Linear Discriminant Analysis 

     13737 samples
     
     19 predictor
      
     5 classes: 'A', 'B', 'C', 'D', 'E' 

      Pre-processing: centered, scaled 
      Resampling: Bootstrapped (25 reps) 

    Summary of sample sizes: 13737, 13737, 13737, 13737, 13737, 13737, ... 

Resampling results

  Accuracy  Kappa  Accuracy SD  Kappa SD
  0.577     0.462  0.00567      0.0072  
 
 
 
# Testing model ussing the test dataset which was created after partitioning the training dataset
plda <- predict(trainmodlda,testingCV)

# Validation Predicting the outcome using the algorithim on the testing dataset
testplda <- predict(trainmodlda,testdata)

THe outcome of the testplda gave me the answer to the prediction value for the problem id from 1 till 20 specified in the test dataset.


[1] D A A A A C C D A C D A E A C B A D A B
Levels: A B C D E

# Confusion Matrix - Testing the out of sample error for the test set that I have created for cross validation

confusionMatrix(testingCV$classe, plda)

onfusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1224  124  151  170    5
         B  234  543  230  113   19
         C  260   84  594   88    0
         D   81  156  197  507   23
         E  112  198  115   81  576

Overall Statistics
                                          
               Accuracy : 0.5852          
                 95% CI : (0.5725, 0.5978)
    No Information Rate : 0.3247          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.473           
 Mcnemar's Test P-Value : < 2.2e-16       

Statistics by Class:

                     Class: A Class: B Class: C Class: D Class: E
Sensitivity            0.6405  0.49140   0.4615  0.52868  0.92456
Specificity            0.8868  0.87531   0.9060  0.90723  0.90384
Pos Pred Value         0.7312  0.47673   0.5789  0.52593  0.53235
Neg Pred Value         0.8369  0.88158   0.8574  0.90815  0.99021
Prevalence             0.3247  0.18777   0.2187  0.16296  0.10586
Detection Rate         0.2080  0.09227   0.1009  0.08615  0.09788
Detection Prevalence   0.2845  0.19354   0.1743  0.16381  0.18386
Balanced Accuracy      0.7636  0.68336   0.6838  0.71795  0.91420


  

