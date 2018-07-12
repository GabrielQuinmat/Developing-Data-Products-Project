Developing Data Products: Course Project
========================================================
author: Gabriel Quintanar
date: July 12, 2018
autosize: true

Overview
========================================================

This presentation is a brief explanation about my course project submission. 
<h>.

The project is a Shiny Application hosted in ShinnyApps.io
- Bullet 1

For the Source Code
- Bullet 2

Project Overview
========================================================

The Project is about predicting survivors in the Titanic Dataset. As it may be guessed, this dataset is the training dataset.



```r
data <- read.csv("train.csv")
summary(data)
```

```
  PassengerId       Survived          Pclass     
 Min.   :  1.0   Min.   :0.0000   Min.   :1.000  
 1st Qu.:223.5   1st Qu.:0.0000   1st Qu.:2.000  
 Median :446.0   Median :0.0000   Median :3.000  
 Mean   :446.0   Mean   :0.3838   Mean   :2.309  
 3rd Qu.:668.5   3rd Qu.:1.0000   3rd Qu.:3.000  
 Max.   :891.0   Max.   :1.0000   Max.   :3.000  
                                                 
                                    Name         Sex           Age       
 Abbing, Mr. Anthony                  :  1   female:314   Min.   : 0.42  
 Abbott, Mr. Rossmore Edward          :  1   male  :577   1st Qu.:20.12  
 Abbott, Mrs. Stanton (Rosa Hunt)     :  1                Median :28.00  
 Abelson, Mr. Samuel                  :  1                Mean   :29.70  
 Abelson, Mrs. Samuel (Hannah Wizosky):  1                3rd Qu.:38.00  
 Adahl, Mr. Mauritz Nils Martin       :  1                Max.   :80.00  
 (Other)                              :885                NA's   :177    
     SibSp           Parch             Ticket         Fare       
 Min.   :0.000   Min.   :0.0000   1601    :  7   Min.   :  0.00  
 1st Qu.:0.000   1st Qu.:0.0000   347082  :  7   1st Qu.:  7.91  
 Median :0.000   Median :0.0000   CA. 2343:  7   Median : 14.45  
 Mean   :0.523   Mean   :0.3816   3101295 :  6   Mean   : 32.20  
 3rd Qu.:1.000   3rd Qu.:0.0000   347088  :  6   3rd Qu.: 31.00  
 Max.   :8.000   Max.   :6.0000   CA 2144 :  6   Max.   :512.33  
                                  (Other) :852                   
         Cabin     Embarked
            :687    :  2   
 B96 B98    :  4   C:168   
 C23 C25 C27:  4   Q: 77   
 G6         :  4   S:644   
 C22 C26    :  3           
 D          :  3           
 (Other)    :186           
```

Logistic Regression Model
========================================================
To create the logistic model, it was need to use only some of the variables and preprocess the data.

```r
data <- data %>% select(Survived, Pclass, Sex, Age, SibSp, Parch,
                                    Fare, Embarked) %>% 
        mutate(Age = replace_na(Age, mean(Age, na.rm = TRUE)))
inTrain <- createDataPartition(y = data$Survived, p = 0.8, list = FALSE)
training <- data[inTrain, ]
testing <- data[-inTrain, ]
model <- glm(Survived ~ ., family = binomial, data = training)
    model <- step(model, direction = "both", trace = FALSE)
    summary(model)
```

```

Call:
glm(formula = Survived ~ Pclass + Sex + Age + SibSp + Fare, family = binomial, 
    data = training)

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-2.7327  -0.6393  -0.4334   0.6201   2.4320  

Coefficients:
             Estimate Std. Error z value Pr(>|z|)    
(Intercept)  4.649268   0.587044   7.920 2.38e-15 ***
Pclass      -1.026983   0.151892  -6.761 1.37e-11 ***
Sexmale     -2.675017   0.217463 -12.301  < 2e-16 ***
Age         -0.036016   0.008462  -4.256 2.08e-05 ***
SibSp       -0.407242   0.124296  -3.276  0.00105 ** 
Fare         0.003738   0.002651   1.410  0.15848    
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 948.95  on 712  degrees of freedom
Residual deviance: 645.80  on 707  degrees of freedom
AIC: 657.8

Number of Fisher Scoring iterations: 5
```

Logistic Plot
========================================================
Finally the AUC Plot from the ROCR Package (This is not showed in the shiny app)

![plot of chunk unnamed-chunk-4](Developing_Data_Products_Course_Project2-figure/unnamed-chunk-4-1.png)

Accuracy: 88.9974737%
