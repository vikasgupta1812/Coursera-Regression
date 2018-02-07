# title: CARS Analysis

#### [Cars Analysis.ipynb](http://nbviewer.jupyter.org/github/vikasgupta1812/Coursera-Regression/blob/master/Cars%20Analysis.ipynb)

This analysis is done to explore a dataset of collection of cars. The main aim is to explore the relationship between a set of variables and miles per galllon. The two questions which needs to be answered are:- 

1. Is an automatic or manual transmission better for MPG. 
2. Quantify the MPG difference between automatic and manual transmissions. 


## Executive summary 
During this analysis it was found that the transmission type (automatic/ manual) has significant effect on the miles per gallong (mpg) of the vehicle. In addition to transmission, two other variables - `weight` and `qsec` have impact on the `mpg` as well. Keeping other variables constant, Manual transmission is approx. `9.62` mpg more than automatic transmission. 



## Analysis

```{.python .input}
%%R
library(ggplot2)
```

load the data.

```{.python .input}
%%R
data(mtcars)
attach(mtcars)
```

convert automatic variable to logical type

```{.python .input}
%%R
mtcars$am  <- as.logical(mtcars$am)
```

summarise automatic and maual cars in the dataset.

```{.python .input}
%%R
summary(mtcars$am)
```

There is no missing value. 

check the summary of mpg variable.

```{.python .input}
%%R
summary(mtcars$mpg)
qplot(mtcars$mpg,binwidth=2)
```

plot boxplot of mpg against the automatic/ manual transmission

```{.python .input}
%%R
p  <- ggplot(mtcars, aes(am, mpg))
p + geom_boxplot()
```

generate the base model with linear regression between mpg and automatic transmission

```{.python .input}
%%R
modFitbase  <- lm(mpg ~ am, data = mtcars)
summary(modFitbase)
```

plot a graph between residual and mpg to see if these two are correlated.

```{.python .input}
%%R
qplot(modFitbase$residuals, mtcars$mpg)
```

The correlation seems to be significant. So Find the correlation between residual and output mpg

```{.python .input}
%%R
cor(modFitbase$residuals, mtcars$mpg)
```

This correlation is significant. we are missing some other important variables in our model.

```{.python .input}
%%R
modFitall  <- lm(mpg ~ ., data= mtcars)
summary(modFitall)
```

By looking at `Pr(>|t|)` value for each variables, we see that `wt`, `am` and `qsec` are three significant variables.

```{.python .input}
%%R
modFit3  <- lm(mpg ~ wt+qsec+am-1, mtcars)
summary(modFit3)
```

We can conclude that `am`(transmission type) has significant influence on `mpg` but 'wt'(weight) and `qsec`(1/4 mile time) also influence `mpg`.

Below are the residual plots to see the fit of data.

```{.python .input}
%%R
plot(density(resid(modFit3 ))) #A density plot
qqnorm(resid(modFit3 )) # A quantile normal plot - good for checking normality
qqline(resid(modFit3 ))
```
