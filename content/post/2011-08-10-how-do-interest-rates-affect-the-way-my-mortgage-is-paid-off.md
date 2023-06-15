---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-08-10T16:29:34Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=46
id: 46
tags:
- Interest rates
- R
- RStudio
title: How do interest rates affect the way my mortgage is paid off?
url: /?p=46
---

With a baby on the way my wife and I have become very interested in the interest rate on our mortgage, and how it might go up or down, and how that will affect whether we overpay to build up some equity, etc. etc. etc. As you will know if you’ve ever thought about your mortgage payments, it’s all quite complicated and difficult to think about.

For a bit of fun I thought I would produce a graph which summarises the value of the mortgage over time as well as the proportion of the money which is spent on capital and interest payments. The repayment is fixed for a given value of interest, so a stacked barchart will have a fixed height, with a different proportion of the bar coming from capital and interest payments over time.

I was going to make it a dynamic graph in which you could change the interest rate with a slider (using the excellent RStudio [manipulate](http://www.rstudio.org/docs/advanced/manipulate) library) but having worked through it it’s a bit more complicated than I thought- I’ll do this in a subsequent post because I’m dying to give the manipulate library a try. For now I’ve produced the code and the accompanying graph based on a mortgage of £150,000 with an interest rate of 4%.

The code, which really is very simple and owes a big debt to [this ](http://www.hughchou.org/calc/formula.html) wonderful post, follows:

```
<pre class="brush: r; title: ; notranslate" title="">
# P = principal, the initial amount of the loan
# I = the annual interest rate (from 1 to 100 percent)
# L = length, the length (in years) of the loan, or at least the length over which the loan is amortized.
# 
# J = monthly interest in decimal form = I / (12 x 100)
# N = number of months over which loan is amortized = L x 12
# Monthly payment = M = P * ( J / (1 - (1 + J) ^ -N))
# 
# Step 1: Calculate H = P x J, this is your current monthly interest
# Step 2: Calculate C = M - H, this is your monthly payment minus your monthly interest, so it is the amount of principal you pay for that month
# Step 3: Calculate Q = P - C, this is the new balance of your principal of your loan.
# Step 4: Set P equal to Q and go back to Step 1: You thusly loop around until the value Q (and hence P) goes to zero. 

# set variables

P = 150000
I = 4
L = 25
J = I / (12 * 100)
N = L * 12
M = P * ( J / (1 - (1 + J) ^ -N))

# make something to store the values in

Capital=numeric(300)
Interest=numeric(300)
Principal=numeric(300)

# loop

for (i in 1:300) {

H= P * J
C= M-H
Q= P- C
P= Q

Capital[i]=C
Interest[i]=H
Principal[i]=Q

}

# plot

par(cex=.7)

barplot(matrix(rbind(Capital[seq(1, 300, 12)], Interest[seq(1, 300, 12)]), ncol=25), xaxt=&quot;n&quot;, yaxt=&quot;n&quot;, col=c(&quot;green&quot;, &quot;red&quot;), xlim=c(1, 33), bty=&quot;n&quot;, main=paste(&quot;Monthly payment £&quot;, round(M), sep=&quot;&quot;))
legend(&quot;topright&quot;, c(&quot;Capital payment&quot;, &quot;Interest payment&quot;), fill=c(&quot;green&quot;, &quot;red&quot;), bty=&quot;n&quot;)

par(new=TRUE)

plot(seq(1, 300, 12), Principal[seq(1, 300, 12)], xlab=&quot;Year&quot;, ylim=c(0,160000), ylab=&quot;Remaining loan amount&quot;, xaxt=&quot;n&quot;, xlim=c(1, 330), bty=&quot;n&quot;)
axis(1, at=seq(1, 300, 12), labels=1:25)

```

And here’s the plot (click to enlarge)!

[![](http://chrisbeeley.net/wp-content/uploads/2011/08/mortgageplot.png?w=300 "MortgagePlot")](http://chrisbeeley.net/wp-content/uploads/2011/08/mortgageplot.png)