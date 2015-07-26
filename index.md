---
title       : Titanic Odds
subtitle    : Data Exploration in Shiny
author      : Rob Moore
job         : Student in Developing Data Products
logo        : PeopleTakeWarningTitanic.jpg
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
github      : {user: robmoore, repo: datadevprod-pitch}
license     : by-nc-sa
---

## Titanic as a Subject

> - Over a century later, still a subject of fascination
    - The 1997 movie was the largest grossing in history for over a decade
    - [World-wide artifact exhibition](http://www.rmstitanic.net/) continues to attract visitors 
> - Popular topic for data analysis
    - Data from the British Board of Trade used in their investigation of the sinking is part of the [R datasets package](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/Titanic.html)
    - Kaggle features a popular [Titanic competition](https://www.kaggle.com/c/titanic) based on a different and more detailed data set

---

## Introducing Titanic Odds

> - Provides users a simple way of exploring the data
> - Users can select from three different passenger characteristics (age, sex, and class)
> - The results feature the prediction that passenger would have survived the Titanic according to our model
> - The user is also shown the actual survival percentages from the data as a point of comparison
> - The difference between the two values (actual and predicted) indicates the limits of the model

---

## Behind the scenes

> - Built using the [Shiny framework](http://shiny.rstudio.com/)
> - The application employees logistic regression to calculate the odds of survival 
$$
\begin{equation*} 
\pi(x) = \frac{1}{1+ e^{-(\beta'X)}} \hspace{1cm} \beta'X = \beta_0 + \beta_1x_1 + \beta_2x_2 + \ldots 
\end{equation*}
$$
> - Uses [glm](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/glm.html) function in R to create model used to perform prediction
> - Calculates the actual percentage of surivors for each combination of characteristics using the 

---

## Example prediction


```r
library(scales)

model <- glm(Survived ~ Class + Sex + Age, 
    data = as.data.frame(Titanic),
    weights = Freq, 
    family = binomial)
pred <- percent(predict(model, 
                        newdata = data.frame(Class = '1st', 
                                             Sex = 'Female', 
                                             Age = 'Adult'), 
                        type = 'response'))
```

 Based on this model, the predicted chance of survival for a female adult 1st class passenger is 88.5%.
 
 However, the actual percent of these passengers who survived based on this data set is 97.2%. 
