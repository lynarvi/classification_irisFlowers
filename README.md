# Classification of Iris Flowers Project in Data Science 2

Classification of Iris Flowers using caret packages machine learning and other syntax in R that i have learn in Data Science 2
Learning Data Science is pretty fun for me, you will get your taste in different approach and statistical configuration in every data sets, Data science is helping people in various socioeconomic and health sectors. Therefore i realize that data and data scientist is there to help the world to become a better place. Learning Data Science in order to create better solutions for real-world problems that people may face today.

## 1. Install packages

    install.packages('caret')

We may need other packages, but caret should ask us if we want to load them. If you are having problems with packages, you can install the caret packages and all packages that you might need by typing:

    install.packages("caret", dependencies=c("Depends", "Suggests"))

Now, lets load the package that we are going to use in this entire project setup

    library(caret)

The caret package provide a consistent interface into hundreds of machine learning algorithms and provides useful convenience methods for data visualization, data resampling, model that need for training and model comparison, among other features. It's a must have tool for machine learing projects in R.

We might stumble upon such error that we never expected, make sure to install also this package.

    install.packages(“ellipse”)

## 2. Load the Data

    `irisDataSet = read.csv("iris.csv", header=TRUE, stringsAsFactors=T)`

**stringsAsFactors=T** to avoid random errors later on

##### 2.3 Create a Validation Dataset

We need to know that the model we created is any good.

Later, we will use statistical methods to estimate the accuracy of the models that we create on unseen data. We also want a more concrete estimate of the accuracy of the best model on unseen data by evaluating it on actual unseen data.
That is, we are going to hold back some data that the algorithms will not get to see and we will use this data to get a second and independent idea of how accurate the best model might actually be.

We will split the loaded dataset into two, 80% of which we will use to train our models and 20% that we will hold back as a validation dataset.

create a list of 80% of the rows in the original dataset we can use for training
`validation_index = createDataPartition(irisDataSet$Species, p=0.80, list=FALSE);`

select 20% of the data for validation
`validation = irisDataSet[-validation_index,];`

use remaining 80% of ata to training and testing the models
`irisDataSet = irisDataSet[validation_index,]`

## 3. Summarize Dataset

We will land into exciting part, now it is time to take a look at the data

In this step we are going to tkae a look at the data a few different ways:

- Dimensions of the dataset
- Types of the attributes
- Peek at the data itself
- Levels of the class attribute.
- Breakdown of the instances in each class
- Statistical summary of all attributes

##### 3.1 Dimensions of Dataset

We can get a quick idea of how many istances (rows) and how many attributes (columns) the data contains with the dim function

    dim(irisDataSet)

The output should be `[1] 120 5` 120 instances and 5 attributes

##### 3.2 Types of attributes

It is also good idea to get an idea of the types of the attributes we are getting from the datasets. They could be doubles, integers, strings, factors etc.

Knowing the types is important as it will give you an idea of how to better summarize the data you have and the types of transforms you might need to use to prepare the data before you model it.

    sapply(irisDataSet, class)`

We should getting this output
`sepal_lengths sepal_width petal_length petal_width Species "numeric" "numeric" "numeric" "numeric" "factor"`

##### 3.3 Peek at the Data

it is also always a good idea to actually have an eyeball to your data.

    head(irisDataSet)

We should getting this out put
`sepal_lengths sepal_width petal_length petal_width Species 1 5.1 3.5 1.4 0.2 setosa 2 4.9 3.0 1.4 0.2 setosa 3 4.7 3.2 1.3 0.2 setosa 4 4.6 3.1 1.5 0.2 setosa 6 5.4 3.9 1.7 0.4 setosa 7 4.6 3.4 1.4 0.3 setosa`

##### 3.4 Levels of the Class

The class variable is a factor. A factor is a class that has multiple class labels or levels. Lets peek into it

    levels(irisDataSet$Species)

We should getting this type of output `[1] "setosa" "versicolor" "virginica"`

This is a multi-class or a multinomial classification problem. If there were two levels, it would be a binary classification problem.

##### 3.5 Class Distribution

Lets now take a look at the number of instances (row) that belong to each class. We can view this as an absolute count and as a percentage

    percentage = prop.table(table(irisDataSet$Species)) * 100; cbind(freq=table(irisDataSet$Species), percentage=percentage)

We should getting this type of output

    freq percentage

    setosa 40 33.33333
    versicolor 40 33.33333
    virginica 40 33.33333
    Confusion Matrix and Statistics

##### 3.6 Statistical Summary

Now finally, we can take a look at a summary of each attribute

This includes the mean, the min and max value as well as some percentiles (25th, 50th, or media and 75th e.g. values at this points if we ordered all the values for an attribute).

    summary(irisDataSet)

We can see that all of the numerical values have the same scale (centimeters) and similar ranges [0,8] centimeters.

    sepal_lengths    sepal_width     petal_length    petal_width
     Min. :4.400 Min. :2.200 Min. :1.000 Min. :0.100
     1st Qu.:5.200 1st Qu.:2.800 1st Qu.:1.600 1st Qu.:0.300
     Median :5.800 Median :3.000 Median :4.400 Median :1.350
     Mean :5.867 Mean :3.058 Mean :3.795 Mean :1.209
     3rd Qu.:6.400 3rd Qu.:3.325 3rd Qu.:5.100 3rd Qu.:1.800
     Max. :7.900 Max. :4.400 Max. :6.900 Max. :2.500
     Species
     setosa :40
     versicolor:40
     virginica :40

## 4. Visualize Dataset

We now have a an idea about our data iris Flowers. We need to extend that with some visualization

We are going to look at two types of plots :

- Univariate plots to better understand each attribute.
- Multivariate plots to better understand the relationships between attributes.

##### 4.1 Univariate Plots

We start with some univariate plots, that is, plots of each individual variable.

it is helpful with visualization to have a way to prefer to just the input attributes and just the output attributes. Let's set that up and call the inputs attribute `x` and the output attribute `y`

    #split input and output

    x = irisDataSet[,1:4];
    y = irisDataSet[,5];

Given that the input variables are numeric, we can create box and whisker plots of each

    #boxplot or each attribute on one image

    par(mfrow=c(1,4))
    for(i in 1:4) {
    boxplot(x[,i], main=names(iris)[i])
    }

This will give us much clearer idea of the distribution of the input attributes

<div align="center">
    <img  width="800" height="800" src="https://github.com/zneret03/classification_irisFlowers/blob/main/static/Boxplot.png">
</div>

We can also create a barplot of the Species class variable to get a graphical representation of the class distribution

    barplot for class breakdown
    plot(y);

<div align="center">
    <img  width="800" height="800" src="https://github.com/zneret03/classification_irisFlowers/blob/main/static/Barplot.png">
</div>

##### 4.2 Multivariate Plots

Now we can take a peek to the interaction between the variables

lets take a look at scatterplots of all pairs of attributes and color the points by class. In addition, because scatterplots show that points for each class ar generally separate, we can draw ellipses around them

    #scatter plot matrix
    featurePlot(x=x, y=y, plot="ellipse");

<div align="center">
    <img  width="800" height="800" src="https://github.com/zneret03/classification_irisFlowers/blob/main/static/Scatter_plot_matrix.png">
</div>

As we can see to the scatterplot matrix, the clear relationshi between the input attributes (trends) and between attributes and the class value (ellipses)

We can also take a look with box and whisker plots for each input variable again, but this time broken down into separate plots for each class. This can help to tease out obvious linear separations between classes

    #box and whisker plots for each attribute
    featurePlot(x=x, y=y, plot="box")

<div align="center">
    <img  width="800" height="800" src="https://github.com/zneret03/classification_irisFlowers/blob/main/static/Box_and_whisker.png">
</div>

Next we can get an idea of the distribution of each attribute, again like the box and whisker plots, broken down by class value. Sometimes histograms are good for this, but in this case we will use some probability density plots to give nice smooth lines for each distribution.

    #density plots for each attribute by class value
    scales = list(x=list(relation="free"), y=list(relation="free"))
    featurePlot(x=x, y=y, plot="density", scales=scales);

<div align="center">
    <img  width="800" height="800" src="https://github.com/zneret03/classification_irisFlowers/blob/main/static/Density_Plots.png">
</div>

Like the boxplots, we can see the differences in distribution of each attribute by class value. We can also see the Gaussian-like distribution (bell curve) of each attribute

## 5. Evaluate Some Algorithms

Now its time to create some models of the data and estimate their accuracy on unseen data, we wil train different models and compare who is the must accurate one.

Here is the step what we will be going to cover :

- Set-up the test harnes to use 10-fold cross validation
- Build 5 different models to predict species from flower measurements
- Select the best model

##### 5.1 Test Harnes

We will use 10-folds cross validation to estimate the accuracy of the model

This will plits the dataset into 10 parts, train in 9 and test on 1 and release all combinitations of train-test splits. We will also repeat the process 3 times for each algorithms with different split of the data into 10 groups, in an effort to get a more accurate estimate.

    #Run algorithms using 10-fold cross validation
    control = trainControl(method="cv", number=10);
    metric = "Accuracy"

We are ysubg metric to evaluate models. This is a ratio of the number of correctly predicted instances in divided by the total number of instances in the dataset multiplied by 100 to give a percentage (e.g 95% accurate). We will be using the metric variable when we build and evaluate each model

##### 5.1 Build Models

We still dont know what algorithm to use on this problem or what configurations to use. We get an idea from the plots that some of the classes are partially linearly separable in some dimensions.

These are the 5 different algorithms that we will be using for testing accuracy

- Linear Discriminant Analysis (LDA)
- Classification and Regression Trees (CART)
- K-Nearest Neighbors (kNN)
- Support Vector Machines (SVM) with linear kernel.
- Random Forest (RF)

This is a good mixture of simple linear (LDA), nonlinear (CART, kNN) and complex nonlinear methods (SVM, RF). We reset the random number seed before reach run to ensure that the evaluation of each algorithm is performed using exactly the same data splits. It ensures the results are directly comparable

These are the models we will building

    a) linear algorithms
    set.seed(7)
    fit.lda = train(Species~., data=irisDataSet, method="lda", metric=metric, trControl=control)

    #b) nonlinear algorithms
    #CART
    set.seed(7)
    fit.cart = train(Species~., data=irisDataSet, method="rpart", metric=metric, trControl=control)

    # kNN
    set.seed(7)
    fit.knn = train(Species~., data=irisDataSet, method="knn", metric=metric, trControl=control)

    #c) advanced algoithms
    #SVM
    set.seed(7)
    fit.svm = train(Species~., data=irisDataSet, method="svmRadial", metric=metric, trControl=control)

    #Random Forest
    set.seed(7)
    fit.rf = train(Species~., data=irisDataSet, method="rf", metric=metric, trControl=control)

##### 5.1 Selecting the best models

So we have 5 models and accuracy estimatins for each. We need to compare the models to each other and select the most accurate one.
We can report on the accuracy of each model by first creating a list of the created models and using the summary function;

    results = resamples(list(lda=fit.lda, cart=fit.cart, knn=fit.knn, svm=fit.svm, rf=fit.rf))
    summary(results)

We can now see the accuracy of each classifier and also the other metrics like kappa :

    Call:
    summary.resamples(object = results)

    Models: lda, cart, knn, svm, rf
    Number of resamples: 10

    Accuracy
            Min.   1st Qu.    Median      Mean 3rd Qu. Max. NA's
    lda  0.9166667 0.9375000 1.0000000 0.9750000       1    1    0
    cart 0.8333333 0.9375000 1.0000000 0.9666667       1    1    0
    knn  0.9166667 0.9166667 0.9166667 0.9500000       1    1    0
    svm  0.8333333 0.9166667 1.0000000 0.9500000       1    1    0
    rf   0.8333333 0.9166667 1.0000000 0.9583333       1    1    0

    Kappa
        Min. 1st Qu. Median   Mean 3rd Qu. Max. NA's
    lda  0.875 0.90625  1.000 0.9625       1    1    0
    cart 0.750 0.90625  1.000 0.9500       1    1    0
    knn  0.875 0.87500  0.875 0.9250       1    1    0
    svm  0.750 0.87500  1.000 0.9250       1    1    0
    rf   0.750 0.87500  1.000 0.9375       1    1    0

We can also create a plot of the model evaluation results and compare the spread and the mean accuracy of each model. There is a population of accuracy measures for each algorithm because each algorithm was evaluated 10 times (10 folds cross validation)

<div align="center">
    <img  width="800" height="800" src="https://github.com/zneret03/classification_irisFlowers/blob/main/static/dotplot.png">
</div>

We can see the most accurate model is LDA whit confidence level of 0.95 or 95%.
We can also print the result of LDA or Linear Discriminant Analysis

    Linear Discriminant Analysis

    120 samples
    4 predictor
    3 classes: 'setosa', 'versicolor', 'virginica'

    No pre-processing
    Resampling: Cross-Validated (10 fold)
    Summary of sample sizes: 108, 108, 108, 108, 108, 108, ...
    Resampling results:

    Accuracy  Kappa
    0.975     0.9625

As we can see the accuracy is 0.975, its pretty high accuracy

## 6. Make Predictions

The LDA was the most accurate model that we have.

This will give us an independent final check on the accuracy of the best model.it is valuable to keep a validation set just incase we made a slip during such as overfitting to the training set or a data leak. Both will result in an overly optimistic result.

We can run the LDA directly on the validation set and summarize the results in confusion matrix.

    #estimate skills of LDA on the validatio dataset
    predictions = predict(fit.lda, validation)
    confusionMatrix(predictions, validation$Species);

We can see that the accuracy of iris flowers dataset classification is 100% or 1. It was a small validation with 20% of the dataset.

    Confusion Matrix and Statistics

            Reference
    Prediction   setosa versicolor virginica
    setosa         10          0         0
    versicolor      0         10         0
    virginica       0          0        10

    Overall Statistics

                Accuracy : 1
                    95% CI : (0.8843, 1)
        No Information Rate : 0.3333
        P-Value [Acc > NIR] : 4.857e-15

                    Kappa : 1

    Mcnemar's Test P-Value : NA

    Statistics by Class:

                        Class: setosa Class: versicolor Class: virginica
    Sensitivity                 1.0000            1.0000           1.0000
    Specificity                 1.0000            1.0000           1.0000
    Pos Pred Value              1.0000            1.0000           1.0000
    Neg Pred Value              1.0000            1.0000           1.0000
    Prevalence                  0.3333            0.3333           0.3333
    Detection Rate              0.3333            0.3333           0.3333
    Detection Prevalence        0.3333            0.3333           0.3333
    Balanced Accuracy           1.0000            1.0000           1.0000

But if we change the confusion matrix data into a full dataset of Iris flowers

    #estimate skills of LDA on the validatio dataset
    predictions = predict(fit.lda, irisDataSet)
    confusionMatrix(predictions, irisDataSet$Species);

We will be getting the 0.98 or 98% percent accuracy of Iris Flowers Dataset Classifier its still pretty high accuracy for predicting Iris flower datasets

    Confusion Matrix and Statistics

                Reference

    Prediction setosa versicolor virginica
    setosa 40 0 0
    versicolor 0 38 0
    virginica 0 2 40

    Overall Statistics

                Accuracy : 0.9833
                    95% CI : (0.9411, 0.998)
        No Information Rate : 0.3333
        P-Value [Acc > NIR] : < 2.2e-16

                    Kappa : 0.975


    Mcnemar's Test P-Value : NA

    Statistics by Class:

                        Class: setosa Class: versicolor Class: virginica

    Sensitivity 1.0000 0.9500 1.0000
    Specificity 1.0000 1.0000 0.9750
    Pos Pred Value 1.0000 1.0000 0.9524
    Neg Pred Value 1.0000 0.9756 1.0000
    Prevalence 0.3333 0.3333 0.3333
    Detection Rate 0.3333 0.3167 0.3333
    Detection Prevalence 0.3333 0.3167 0.3500
    Balanced Accuracy 1.0000 0.9750 0.9875

## Technology & packages used

- [R](https://www.r-project.org/)
- [Caret](https://cran.r-project.org/web/packages/caret/caret.pdf)
