---
title       : Black & Scholes
subtitle    : European option calculator
author      : jerplo
job         : 
framework   : revealjs      # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

### Black & Scholes shine in Shiny

The Black-Scholes model is used to calculate the theoretical price of European 
put and call options. It was published by Fischer Black and Merton Scholes 
back in 1973. It's not the most comprehensive model to price options, but
despite its limitations it is still used today for 'quick and dirty' option 
calculations. Calculating it by hand can be a bit of a chore still though, hence 
why I devised a Shiny app as a quick introduction to option pricing using the model.
<br>
In the next few slides, both the mathematical model and the function in R will
be outlined. After the dry bit and a quick glimpse 'under the hood', it's time 
to get busy with the app yourself. 
<br>
Enjoy!

--- .class #id 

### The Model

$call = SN(d_1) - Xe^{-rt}N(d_2)$  
$put = Xe^{-rt}N(-d_2) - SN(-d_1)$  
<br>
- S, the spot (or current) price of the underlying asset  
- X, the strike (or exercise) price of the option  
- r, the risk free interest rate  
- t, the option's maturity date  
- $\sigma$, the volatility of the underlying instrument  
- N, cumulative normal standard distribution  
- $d_1 = \dfrac{ln \left( \dfrac{S}{X} \right) + \left(r + \dfrac{\sigma^2}{2}\right)t}{\sigma \sqrt{t}}\\$ 
- $d_2 = d_1 - \sigma \sqrt{t}$

--- .class #id 

### Black & Scholes function in R
the Black Scholes formula translated into R code:


```r
blackscholes <- function(S, X, r, t, sigma) {
        values <- NULL
        
        d1 <- (log(S/X)+(r+sigma^2/2)*t)/(sigma*sqrt(t))
        d2 <- d1 - sigma * sqrt(t)
        
        values[1] <- S*pnorm(d1) - X*exp(-r*t)*pnorm(d2)
        values[2] <- X*exp(-r*t) * pnorm(-d2) - S*pnorm(-d1)
        names(values) <- c("call", "put")
        round(values, 4)
}
```

--- .class #id 

### Black Scholes example
An example of the R code in use: spot price is 50, strike price is 55, the 
risk free rate is 6%, the time to maturity is 90 days and the volatility of 
the underlying asset is 32%.


```r
S <- 50 
X <- 55 
r <- 0.06 # in decimals
sigma <- 0.32 # in decimals
t <- 90/365 # t as a fraction of a year
optionprices <- blackscholes(S, X, r, t, sigma)
optionprices
```

```
##  call   put 
## 1.616 5.809
```

--- .class #id 

### Try it for yourself
Now that you've seen the inner workings, play with the model yourself:
http://jerplo.shinyapps.io/DataProductsAssignment/

--- .class #id



