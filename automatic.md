The gearbox and miles per gallon
======================================

**Executive Summary**

The following analysis looks at the issue of whether cars with manual gearboxes have better fuel economy, as measured in miles per gallon (mpg), then those with automatic transmission. On the surface it appears that cars with manual transmission do have better fuel economy than those with automatic transmission.  On average, cars in the sample with manual transmission had an mpg 7.25 greater than automatics. However the gearbox is not the only thing to consider.  There may be other features of a car that influence fuel economy.  It is possible, for example, that weight is more important than transmission.  So when analyzing the impact of the gearbox, one has to consider other factors. An approach called "hierarchical multiple regression" was undertaken.  The question being asked was how much did the gearbox impact mpg, once other factors had been considered.  Using this approach, no evidence was found that manual transmission cars had better fuel economy than automatic ones.

**Methodology and descriptives**

The analysis was based on a sample of 32 cars.  Nineteen were manual, thirteen were automatic.  Seven were of the Mercedes brand. The mean mpg was 20.1, the standard deviation 6.03. A histogram for mpg (Figure 1) suggested that it was not normally distributed. The main indepedent variable was transmission, which had two dummy-coded levels: 0 for automatic, 1 for manual. Other independent variables were considered, that might act as confounders to mpg. Vehicle weight was clearly important, because the heavier a vehicles weighs, the greater its fuel consumption. The mean weight, in thousands of pounds, was 3.22, with a standard deviation of 0.98.

Horsepower (HP) was another independent variable (mean 146.69, SD 68.56); so was the number of cylinders (mean 6.19, SD 1.79).


**Analysis**

A regression was run, with mpg as the dependent variable and transmission as the predictor variable (see Figure 2 for the scatterplot).

```r
model <- lm(mtcars$mpg ~ mtcars$am)
summary(model)
```

```
## 
## Call:
## lm(formula = mtcars$mpg ~ mtcars$am)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -9.392 -3.092 -0.297  3.244  9.508 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    17.15       1.12   15.25  1.1e-15 ***
## mtcars$am       7.24       1.76    4.11  0.00029 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.9 on 30 degrees of freedom
## Multiple R-squared:  0.36,	Adjusted R-squared:  0.338 
## F-statistic: 16.9 on 1 and 30 DF,  p-value: 0.000285
```
The regression was statistically significant, F(30,1)=16.9, p<0.001.  R-squared was 0.36, so transmission was able to explain 36% of the variance in mpg. The model was: Y(estimated) = b0(=17.15) + b1(7.24). As b0 is the constant, of 17.15, the model simply adds 7.24 mpg if a car has manual transmission (ie coded at 1, rather than 0 for automatic).  The standard error of b1, for transmission, is 1.76, so the 95% confidence interval (df=30) 

As far as diagnostics are concerned, the predicted mpg was plotted against the residual.  As there was a single, binary independent variable, there were only two predicted values - an automatic was predicted to have an mpg of 17.15, a manual 24.39.  The variance of the residuals for manuals was wider than that of transmissions, and this suggests a certain amount of heteroskedasticity.  

The first stage of the second model had mpg as the dependent variable, and weight, HP and cylinders as the predictor variables.  It was statistically significant, F(3,28)=50.2, p<0.001, R-Squared=0.843. Weight was the only predictor having a significant impact, t=-4.28.  At the second stage, transmission was added:

```r
model2 <- lm(mpg ~ wt + hp + cyl + am,mtcars)
summary(model2)
```

```
## 
## Call:
## lm(formula = mpg ~ wt + hp + cyl + am, data = mtcars)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -3.476 -1.847 -0.554  1.276  5.661 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  36.1465     3.1048   11.64  4.9e-12 ***
## wt           -2.6065     0.9198   -2.83   0.0086 ** 
## hp           -0.0250     0.0136   -1.83   0.0786 .  
## cyl          -0.7452     0.5828   -1.28   0.2119    
## am            1.4780     1.4411    1.03   0.3142    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.51 on 27 degrees of freedom
## Multiple R-squared:  0.849,	Adjusted R-squared:  0.827 
## F-statistic:   38 on 4 and 27 DF,  p-value: 1.02e-10
```
On the second model, with transmission added, R-squared had increased to 0.849, from 0.843.  This increase is not statistically significant, F(1,27)=1.05,p=0.314.  In the second multiple regression, the t value for transmission was 1.03,p=0.314.

**Conclusion**

Transmission appears to have a significant impact on mpg, when entered into a regression model on its own.  However when other factors are entered, in particular vehicle weight, any significant impact of transmission disappears. 







```r
hist(mtcars$mpg, xlab="Mpg", ylab="Frequency",
     breaks=10, col="red")
```

![Histogram for mpg](figure/unnamed-chunk-3.png) 

```r
plot(mtcars$am,mtcars$mpg,xlab="Transmission, 0=auto, 1=man",
     ylab="Mpg",pch=16,col="blue")
abline(lm(mtcars$mpg ~ mtcars$am))
```

![Scatterplot for transmission and mpg](figure/unnamed-chunk-4.png) 


```r
plot(predict(model),resid(model),pch=16,col="blue",
     xlab="Predicted mpg",ylab="Residual")
abline(h=0,col="red")
```

![Predicted mpg against the residuals](figure/unnamed-chunk-5.png) 
