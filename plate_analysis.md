Phytophthora
================
Sam Muir
2022-12-16

## Initial data cleaning and plotting

I filtered the data to only include the counts after 4 days for both
trials, as well as only including the 10-1 dilution. Then, I plotted the
data to check for normal distribution. The data appears to be skewed.
![](plate_analysis_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Log transforming the data shows a more normal distribution for some of
them? Newtowne Neck is still skewed.

![](plate_analysis_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Plotting Averages

``` r
ggplot(averages, aes(location, `mean(log_cfu)`, fill = salinity_ppt)) +
  geom_bar(position="dodge", stat = "identity", size = 0.8) +
  geom_errorbar(aes(ymin=`mean(log_cfu)`-`sd(log_cfu)`, 
                    ymax=`mean(log_cfu)`+`sd(log_cfu)`), 
                width=0.2, linewidth = 0.5,
                 position=position_dodge(.9)) +
  theme_linedraw() +
  scale_x_discrete(labels=c("CM" = "Chapman", "CP" = "Chapel Point",
                              "NN" = "Newtowne Neck", "PL" = "Point Lookout")) +
  labs(x = "", y = "Log of Mean cfu/mL", fill = "Salinity (ppt)") +
  scale_fill_viridis(end = 0.8)
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.

![](plate_analysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## Statistical Analysis

Levene’s Test

``` r
leveneTest(log_cfu ~ location, averages)
```

    ## Warning in leveneTest.default(y = y, group = group, ...): group coerced to
    ## factor.

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value    Pr(>F)    
    ## group  3   26.53 3.569e-07 ***
    ##       20                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Since p \> 0.05 there is equal variance and we can perform an ANOVA.

ANOVA

``` r
aov <- aov(log_cfu ~ location, averages)
summary(aov)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## location     3  7.167  2.3889   12.97 6.25e-05 ***
    ## Residuals   20  3.683  0.1841                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
