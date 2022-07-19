Fourth Blog Post
================
Xin Wang
2022-07-19

``` r
rmarkdown::render(
    '/Users/ceciliawang/Desktop/ST558/git/xinceciliaw.github.io/_Rmd/2022-07-19-fourth-blog-post.Rmd', 
    output_format = "github_document",
    output_dir = "/Users/ceciliawang/Desktop/ST558/git/xinceciliaw.github.io/_posts",
    output_options = list(
      html_preview = FALSE
    )
)
```

## Method I find most interesting

## Fit the Model

``` r
library(tidyverse)
library(caret)
```

``` r
set.seed(1)
trainIndex <- createDataPartition(iris$Species, p =0.6, list = FALSE)
Train<- iris[trainIndex,]
Test<- iris[-trainIndex,]
```

``` r
fitknn <- train(Species ~ . ,
                data = Train,
                method = "knn",
                trControl = trainControl(method = 'cv', 
                                         number = 10),
                preProcess = c("center", "scale"),
                tuneGrid = data.frame(k = 1:15))
fitknn
```

    ## k-Nearest Neighbors 
    ## 
    ## 90 samples
    ##  4 predictor
    ##  3 classes: 'setosa', 'versicolor', 'virginica' 
    ## 
    ## Pre-processing: centered (4), scaled (4) 
    ## Resampling: Cross-Validated (10 fold) 
    ## Summary of sample sizes: 81, 81, 81, 81, 81, 81, ... 
    ## Resampling results across tuning parameters:
    ## 
    ##   k   Accuracy   Kappa    
    ##    1  0.9666667  0.9500000
    ##    2  0.9444444  0.9166667
    ##    3  0.9555556  0.9333333
    ##    4  0.9444444  0.9166667
    ##    5  0.9444444  0.9166667
    ##    6  0.9444444  0.9166667
    ##    7  0.9222222  0.8833333
    ##    8  0.9333333  0.9000000
    ##    9  0.9444444  0.9166667
    ##   10  0.9555556  0.9333333
    ##   11  0.9333333  0.9000000
    ##   12  0.9444444  0.9166667
    ##   13  0.9555556  0.9333333
    ##   14  0.9555556  0.9333333
    ##   15  0.9222222  0.8833333
    ## 
    ## Accuracy was used to select the optimal model using the largest value.
    ## The final value used for the model was k = 1.

``` r
confusionMatrix(data = Test$Species, reference = predict(fitknn, newdata = Test))
```

    ## Confusion Matrix and Statistics
    ## 
    ##             Reference
    ## Prediction   setosa versicolor virginica
    ##   setosa         20          0         0
    ##   versicolor      0         17         3
    ##   virginica       0          4        16
    ## 
    ## Overall Statistics
    ##                                           
    ##                Accuracy : 0.8833          
    ##                  95% CI : (0.7743, 0.9518)
    ##     No Information Rate : 0.35            
    ##     P-Value [Acc > NIR] : < 2.2e-16       
    ##                                           
    ##                   Kappa : 0.825           
    ##                                           
    ##  Mcnemar's Test P-Value : NA              
    ## 
    ## Statistics by Class:
    ## 
    ##                      Class: setosa Class: versicolor Class: virginica
    ## Sensitivity                 1.0000            0.8095           0.8421
    ## Specificity                 1.0000            0.9231           0.9024
    ## Pos Pred Value              1.0000            0.8500           0.8000
    ## Neg Pred Value              1.0000            0.9000           0.9250
    ## Prevalence                  0.3333            0.3500           0.3167
    ## Detection Rate              0.3333            0.2833           0.2667
    ## Detection Prevalence        0.3333            0.3333           0.3333
    ## Balanced Accuracy           1.0000            0.8663           0.8723
