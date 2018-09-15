---
layout: default
title: Socratic Regression
---

# Socratic Regression

## What’s Socratic Regression?

It is an attempt to develop an intuitive understanding of linear regression using Socratic questioning. The starting point for this project was a <a href="https://web.archive.org/web/20040213071852/http://emlab.berkeley.edu/GMTheorem/index.html">geometric proof of the Gauss-Markov theorem</a> which appealed to me as a visual learner. While examining it, I found myself running through lines of questioning in my head which made me realize that Socratic questioning is a more natural way to learn the subject than the traditional ground-up, proof-based approach.

## What’s this Gauss-Markov theorem you speak of? How is it relevant to linear regression?

The Gauss-Markov theorem is a good jump-off point from which to learn about linear regression. There are many ways to fit a line through data, of which ordinary least squares (OLS) is the most well-known. (What! I didn’t realize there are other ways to fit a line?! What are some examples? Generalized least squares (GLS), maximum likelihood estimation (MLE), etc.) The Gauss-Markov theorem explains that under certain conditions, OLS is the “best” method to use.

This is interesting in two ways. First, it helps us understand why everyone seems to like using OLS when doing linear regression. Second, like a big fish in a small pond, OLS is only best in a very narrow sense — it requires several strong conditions and is only best in a small class of methods. By tearing down the various conditions and probing why it can’t compete outside of its class, we end up with a good understanding of linear regression.

## So what does it mean for OLS to be “best”? And what the conditions are required for this?

Recall, the problem of linear regression says: Suppose we have data that is generated in the secret control room of the universe** by the function $$X\beta + \epsilon$$. What is the best way to find $$\beta$$? Remember that as mere mortals, we can never peek into the secret control room of the universe (SCRU). We can only observe $$y$$, final result of $$X\beta + \epsilon$$, and $$X$$ which are our inputs. $$\beta$$ is top secret so we can’t just read it off our observations, and $$\epsilon$$ is a random variable which prevents us from deducing \beta from what we know. We are forced to guess what it is and this process of guessing parameters of underlying models is known in statistics as “estimation”.

The Gauss-Markov theorem says: This problem is way too hard. I can’t tell you the best estimator for $$\beta$$ in all cases; but for the cases where the noise $$\epsilon$$ is distributed with $$\mathbb{E}(\epsilon) = 0$$ and $$\mathbb{Var}(\epsilon) = \sigma^2 I$$, then I can tell you that OLS is the best among the class of *linear*, *unbiased* estimators.

[Some people familiar with regression may ask: Don’t we also need the condition that the data is correctly modeled by $$X\beta$$? In my view, this condition is unnecessary because it is part of the setup of the problem.]

---—

We start with a 3 dimensional space with a plane in it. The plane represents the $$X$$ matrix.

<img src="img/xplane.png">

## What is $$X$$ and why is it represented by a plane?

One superficial view of the $$X$$ matrix is as a store of data where each row stores the covariates of a single data point. $$X$$ is tall and skinny because it must have at least as many data points as it has covariates. This is reminiscent of what we know from high school algebra about systems of equations. Each equations provides information about the relationship between the inputs and outputs, and if we have more unknowns than we have equations, we won’t have enough information to find a unique solution for the unknowns.

[diagram showing shapes of y = X\beta]

We can also view $$X$$ as a linear transformation since we can feed it a vector and have it spit out another. More concretely, it takes in a $$\beta$$ vector and uses the vector’s components as weights for building a linear combination of its columns. This linear combination is then output as the vector of predictions $$y$$. I like to think of $$X$$ as a fridge where each column is an ingredient. $$\beta$$ is a recipe which tells us how much of each ingredient to mix together.

$$
y = X \beta = \beta_1 \begin{bmatrix} | \\ x_1 \\ | \end{bmatrix} + \beta_2 \begin{bmatrix} | \\ x_2 \\ | \end{bmatrix} + … + \beta_i \begin{bmatrix} | \\ x_i \\ | \end{bmatrix}
$$

This leads us to a third geometric view of X. Since X is a transformation, we can visualize it as the space of all its possible outputs. At first you might wonder: There are so many possible outputs of X. How do I visualize this? It turns out that linear algebra has good tools for this. Recall that the space of all possible outputs is the space of all possible linear combinations of X’s columns. In the language of linear algebra, we call this the space _spanned_ by the column vectors, or the column space for short.

<img src="img/xspan.png">

Since $$X$$ is tall and skinny, it has a few tall column vectors. This tells us that the column vectors live in a high-dimensional space and that there are not enough of them to span it. Therefore, the column space is a subspace of the main space. Since we’re limited to imagining the main space in 3 dimensions, the column space of $$X$$ is best represented by a 2 dimensional subspace, a plane.


## Alright, so we have this plane. How does the problem of linear regression look like in this picture?

Recall that the $$X$$ plane consists of all possible outputs of $$X$$ including the output of the One True $$\beta$$. This $$\beta$$ was generated in the SCRU so we can’t observe the vector it produces. What we do observe comes one step later after obscuring noise, $$\epsilon$$, is added.

[diagram showing vector X\beta, label it as unobservable, show vector \epsilon added to it, and label X\beta + \epsilon as observable]

The problem of linear regression is to estimate a $$\hat{\beta}$$ which produces an $$X\hat{\beta}$$ as close as possible to $$X\beta$$, based on the $$X\beta + \epsilon$$ that we observe. In case it isn’t clear, $$\hat{\beta}$$, pronounced “beta hat”, uses the standard “hat notation” to show that it is an estimator of the SCRU value.

[diagram showing vector X\beta and X\hat{\beta}, mark what is observable and what is not]

That's not all. This picture is incomplete because it only shows one observation of $$y$$.Remember that $$y$$ consists of the pure, consummate $$X\beta$$ combined with a chaotic $$\epsilon$$. Since $$epsilon$$ is a random variable, each time we observe $$y$$, it gives a different value. We don't want to find a method of estimation that only works well on a lucky realization of $$\epsilon$$, we want it to work well on aggregate.

The solution is to consider the entire distribution of $$\epsilon$$. Each point in this cloud is a possible observation of $$y$$. Here, we've modeled $$\epsilon$$ as a 3 dimension gaussian.

<img src="img/cloud.png">

This is still not good enough. Clouds are not so great to reason about because they are irregular and don't have nice boundaries. Instead, let's abstract the randomness away by using confidence intervals. The sphere below is essentially a 3 dimensional confidence interval that works just as well to help us understand where the $$y$$s might lie.

<img src="img/sphere.png">

When we were working with a single observation of $$y$$, the problem of linear regression was just about mapping that it onto the plane as closely as possible to $$X\beta$$. Now that we are working with a sphere of $$y$$s, we can restate the problem more generally to be a way to mapping the sphere on to the plane so that the resulting area is as closely bunched around $$X\beta$$ as possible.


## Presumably, OLS is one of the ways to estimate a $$\hat{\beta}$$. How does does the OLS estimate look like?

Pictorially, the OLS estimate is the orthogonal projection of y onto to the X plane.

[diagram showing X\hat{\beta}_{OLS} as the orthogonal projection of y onto the X plane]

It’s easy to see why this is the case. $$\hat{\beta}_{OLS}$$ is derived by minimizing the squared distance between $$y$$ and $$X\hat{\beta}_{OLS}$$. Given a $$y$$ vector and the plane $$X$$, and since the squared distance is basically the standard cartesian distance that we’re familiar with, the nearest point on $$X$$ is $$y$$ is clearly the orthogonal projection.

  [diagram zoomed in, showing y vector above, plane below, show that the point directly below is is the nearest]

## Now we know how the OLS estimator looks like in the picture. Is there a geometric way to see why it is the BLUE estimator?
