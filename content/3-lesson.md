---
title: Generalized linear mixed models
nav: Day 3
topics: Non-normal data; Repeated Measures; Wrap-up
---


{% capture text %}


# Day 3: Generalized Linear Mixed Models - GLMMs

Learning objectives:

-   What are GLMMs

-   What is a distributional assumption

-   How to fit a GLMM

$$
\\[0.2in]
$$

## What are GLMMs

-   Generalized Linear Models are models in which we can assume different distributions for our data beyond the Normal distribution.

-   Similar to general linear models, GLMs can also have random effects, thus, Generalized Linear Mixed Models - GLMMs.

$$
\\[0.2in]
$$

### The structure of a GLMMs

Remember that for a **LMMs**, assuming $\mathbf{y}$ arises from a normal distribution, we have:

$$
\mathbf{y = X\beta + Zu + \epsilon} \\ \mathbf{\begin{bmatrix} u \\ \epsilon \end{bmatrix} \sim \begin{pmatrix}  \begin{bmatrix} 0 \\ 0 \end{bmatrix}, \begin{bmatrix} G \; 0 \\ 0 \; R \end{bmatrix} \end{pmatrix}}
$$

In which:

-   $\mathbf{X\beta}$ represents our fixed effects, where $\mathbf{X}$ is a matrix containing our explanatory variables and $\mathbf{\beta}$ a vector containing the fixed-effects parameters.

-   $\mathbf{Zu}$ represents our random effects, where $\mathbf{Z}$ is a design matrix and $\mathbf{u}$ is the vector containing the random effects parameters.

-   $\mathbf{\epsilon}$ is the vector containing the residuals.

-   From $\mathbf{Zu + \epsilon}$ we have: $\mathbf{G}$ is the variance-covariance matrix of the random effects, and $\mathbf{R}$ is the variance-covariance matrix of the residuals.

    -   $\mathbf{G = \sigma^2_uI}$ and $\mathbf{R = \sigma^2I}$, in which $\mathbf{I}$ is the identity matrix.

Which is similar to:

$$
\mathbf{u} \sim N(0, I\sigma^2_u) \\
\mathbf{\epsilon} \sim N(0, I\sigma^2)
$$

In this case:

$$
E(\mathbf{y}) = \mathbf{X\beta}, \\
Var(\mathbf{y}) = \mathbf{ZGZ' + R}
$$

We can also write this model as:

$$
\mathbf{y} \sim N(\mathbf{X\beta}, \; \mathbf{ZGZ' + R})
$$

or:

$$
\mathbf{y} \sim N(\mathbf{X\beta}, \; \mathbf{\Sigma}) \\
\mathbf{\Sigma} = \mathbf{ZGZ' + R}
$$

For **GLMMs** the structure changes based on the distribution we will assume for $\mathbf{y}$, but is very similar to the last notation presented. A generic definition would be:

$$
\mathbf{y|u} \sim P(\mu, \; \phi)
$$

In which:

-   Linear predictor: $g(\mu) = \eta = \mathbf{X\beta + Zu}$

    -   $g(\mu) = \eta$ is the link function applied to the expected value.

    -   $E(\mathbf{y|u}) = \mu$.

$$
\\[0.2in]
$$

### Components of GLMMs

#### Link Functions

Our linear predictor $\mathbf{X\beta}$ can produce all possible values in the y-axis of a plot, from $- \; \infty$ to $+ \; \infty$ depending on the value of the predictor variable. A link function links the linear predictor and the distribution assumed for the data $\mathbf{y}$.

In the **link scale**, the mean of $\mathbf{y}$ respect linearity of the linear predictor. In the **response scale**, the mean $\mathbf{\mu}$ is back transformed by the inverse link and respects the support of the distribution.

The link function is applied to the expected value ($E(\mathbf{y})$), and not to the observations. Transformation of the observations also effect the error, while link functions only affect the parameters controlling the expected value.

Example of link functions:

| Link Function | Equation | Use | Why |
|:----------------:|:----------------:|:----------------:|:----------------:|
| **Identity Link** | $g(\mu) = \mu$ | Normal dist. | $E(\mathbf{y})$ can take any real value ($-\infty, \; +\infty$) |
| **Logit Link** | $g(\mathbf{\mu}) = log(\frac{\mu}{1-\mu})$ | Logistic, Beta, Binomial dist. | $E(\mathbf{y})$ can take any values between 0 and 1. Maps $(0, \; 1) \rightarrow (-\infty, \; +\infty)$ |
| **Log Link** | $g(\mu) = log(\mu)$ | Poisson, Gamma dist. | $E(\mathbf{y})$ can take any positive values ($\mu > 0$) |

$$
\\[0.2in]
$$

#### Distributional assumption for the data

GLMMs support different distributions from the exponential family. Distributions from the exponential family share common structure, but are relatively different among themselves.

-   **Assumption**: Something you take as true about your data or about the process that generated it!

Important distributions to know are:

-   **For continuous data**: Normal, t, Gamma, Beta.

-   **For discrete data**: Binomial, Poisson, Negative Binomial.

$$
\\[0.2in]
$$

<center>

#### **Normal distribution**

</center>

$$
y \sim N(\mu, \; \sigma^2)
$$

$$
E(y) = \mu \\
Var(y) = \sigma^2
$$

**Support:**

$$
y \in (-\infty, \; +\infty)
$$

## Wrap-up - Why hierarchical models are more important than ever  

### Why are mixed models sometimes called 'hierarchical' or 'multilevel' models?  

{% include figure.html img="day3/hierarchical_ag.jpg" alt="" caption="" width="80%" %}

{% capture figure_content_hierarchical_swine %}
{% include figure.html img="day3/hierarchical_swine.jpg" alt="" caption="Schematic diagram of a Hierarchical Model with repeated measures and subsampling. The variance components portrayed here correspond to a designed experiment disposed in a randomized complete block design (i.e., room as blocking factor, $$\sigma^2_u$$ or $$\sigma^2_{rooms}$$). Pens are the experimental units but there is subsampling (thus adding the variance component $$\sigma^2_v$$ or $$\sigma^2_{pens}$$): pig is the observational unit, and there are 3 pigs per pen being observed. In addition, each pig is observed at several points in time, thus adding the variance component $$\sigma^2_w$$ (or $$\sigma^2_{pigs}$$). Last, $$\sigma^2_e$$ is the residual variance." width="100%" %}
{% endcapture %}
{% include modal.html button="Example of hierarchical models with animals and subsampling" color="success" id="modal-crd-hierarchical-animal" title="Hierarchical Model Animal Example" text=figure_content_hierarchical_swine %}

### Major benefits we get from mixed models   

- Information is **shared** across groups  
- More robust under unbalanced scenarios  
- Very helpful to handle missing data  
- No need to average across observations - information is preserved!  

>I want to convince the reader of something that appears unreasonable: 
*multilevel regression deserves to be the default form of regression.* 
Papers that do not use multilevel models should have to justify 
not using a multilevel approach. Certainly some data and contexts do
not need the multilevel treatment. But most contemporary studies in the social and natural
sciences, whether experimental or not, would benefit from it. Perhaps the most important
reason is that even well-controlled treatments interact with unmeasured aspects 
of the individuals, groups, or populations studied. 
This leads to variation in treatment effects, in which individuals or groups vary
in how they respond to the same circumstance. Multilevel models attempt to quantify
the extent of this variation, as well as identify which units in the data
responded in which ways.  
>
--[Statistical Rethinking, Richard McElreath](https://civil.colorado.edu/~balajir/CVEN6833/bayes-resources/RM-StatRethink-Bayes.pdf).

[[Also see McEreath's blog post](https://elevanth.org/blog/2017/08/24/multilevel-regression-as-default/)]

------

## What's next  

- I'll be teaching STAT 720 this summer and STAT 870 this fall. 
- Feel free to reach out with questions/concerns/more advanced questions.  
- Please answer this [survey](https://forms.gle/Tm9rnzgSVDcVLAik7) to help me improve future editions of the same workshop/create a follow-up based on demand. 

