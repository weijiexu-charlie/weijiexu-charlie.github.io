---
layout: page
permalink: /resources/basics/
title: Information Theory Basics
description: 
nav: false
---

<p class="page-note">These are my personal study notes based on Chapter 2 of <em>Elements of Information Theory</em> (Cover &amp; Thomas, 2nd ed.). All errors are my own. Please refer to the original text for complete and authoritative treatment.</p>


<span class="back-link">
  <i class="fa-solid fa-arrow-left"></i>
  <a href="{{ '/resources/' | relative_url }}">Back to Resources</a>
</span>


---


<!-- <p>&nbsp;</p> -->

<h5>
  <span class="header-underline">ENTROPY</span>
</h5>

The entropy $$H(X)$$ measures the uncertainty in a random variable $$X$$. For a discrete variable, it is defined by

$$
H(X) = -\sum_x p(x) \log p(x) = \mathop{\mathbb{E}}\left[-\log p(x)\right].
$$

Unless otherwise specified, the base of $$\log$$ is 2, and the entropy is measured in *bits*. Since $$-\log p(x) \geq 0,$$ entropy is always non-negative, a property you will commonly see in the mathematical derivation of many papers.


<!-- <p>&nbsp;</p> -->

<h5>
  <span class="header-underline">JOINT ENTROPY, CONDITIONAL ENTROPY</span>
</h5>

Now we have two random variables $$X$$ and $$Y$$. Their *joint entropy* $$H(X, Y)$$ is defined based on their joint probability distribution:

$$
\begin{aligned}
H(X, Y) &= - \sum_x \sum_y p(x, y) \log p(x, y)\\
&= -\mathop{\mathbb{E}} \left[\log p(x, y)\right].
\end{aligned}
$$

Similarly, the *conditional entropy* $$H(Y \mid X)$$ is defined based on conditional probability:

$$
\begin{aligned}
H(Y \mid X) &= \sum_x p(x) H(Y \mid X=x)\\
&= -\sum_x p(x) \sum_y p(y \mid x) \log p(y \mid x) \\
&= -\sum_x \sum_y p(x, y) \log p(y \mid x) \\
&= -\mathop{\mathbb{E}} \left[\log p(y \mid x)\right].
\end{aligned}
$$

There is also a chain rule for joint and conditional entropy. But unlike the chain rule for raw probabilities, the chain rule for entropy is summation instead of multiplication, since entropy operates in logarithmic:

$$
H(X, Y) = H(X) + H(Y \mid X).
$$

Similar to the raw probability, when $$X$$ and $$Y$$ are independent:

$$
H(X, Y) = H(X) + H(Y) \qquad \text{if } X \perp Y.
$$

**Information doesn't hurt**, so conditioning can only reduce entropy:

$$
H(Y \mid X) \leq H(Y).
$$



<!-- <p>&nbsp;</p> -->

<h5>
  <span class="header-underline">RELATIVE ENTROPY (KL DIVERGENCE)</span>
</h5>


The *relative entropy* $$D_{\text{KL}}(p \parallel q)$$ (or sometimes called *KL divergence*) measures how inefficient it is to assume that distribution is $$q$$ when the true distribution is $$p.$$[^true-dist] It is defined as

$$
\begin{aligned}
D_{\text{KL}}(p \parallel q) &= \sum_x p(x) \log \frac{p(x)}{q(x)} \\
&= \underbrace{-\sum_x p(x)\log q(x)}_{\text{cross-entropy } H(p, q)} - \underbrace{\left[-\sum_x p(x) \log p(x) \right]}_{H(p)}.
\end{aligned}
$$

As shown above, relative entropy $$D_{\text{KL}}$$ can be decomposed into the cross-entropy $$H(p, q)$$ and the entropy of the true distribution $$H(p)$$. So relative entropy can be interpreted as the *additional* code length that is needed if the true distribution $$p$$ is encoded as if it were $$q.$$

- It is useful to view $$D_{\text{KL}}(p \parallel q)$$ as the distance between two distributions. But it is not a true distance metric since it is not symmetrical. (See [this page](https://statproofbook.github.io/P/kl-nonsymm) for the proof)
- Relative entropy is non-negative: $$D_{\text{KL}}(p \parallel q) \geq 0.$$ The equality holds iff $$p(x) = q(x)$$ for all $$x.$$ (See [this page](https://statproofbook.github.io/P/kl-nonneg2) for the proof)



<!-- <p>&nbsp;</p> -->

<h5>
  <span class="header-underline">MUTUAL INFORMATION</span>
</h5>

The *mutual information* $$I(X; Y)$$ measures how much information the random variable $$X$$ provides about $$Y.$$ It is a special case of relative entropy. That is, it is the relative entropy between the joint distribution $$p(x, y)$$ and the product distribution $$p(x) p(y)$$. We can view it as the additional information we get if we consider $$X$$ and $$Y$$ are not independent compared to if they were independent.

$$
\begin{aligned}
I(X; Y) &= \sum_x \sum_y p(x, y) \log \frac{p(x, y)}{p(x) p(y)}\\
&= D_{\text{KL}} \left( p(x,y) \parallel p(x)p(y) \right).
\end{aligned}
$$

Another way to interpret $$I(X; Y)$$ is that it measures the information of $$X$$ if $$Y$$ is already given. That is, it can be considered the reduction of uncertainty if one of the variable is already known:

$$
I(X; Y) = H(X) - H(X \mid Y).
$$

Importantly, mutual information is symmetrical:

$$
I(X; Y) = I(Y; X)
$$

Another useful quantity is *conditional mutual information* $$I(X; Y \mid Z)$$ of $$X$$ and $$Y$$ given $$Z:$$

$$
I(X; Y \mid Z) = H(X \mid Z) - H(X \mid Y, Z).
$$

There is also a chain rule for mutual information:

$$
I(X, Y; Z) = I(X; Z) + I(Y; Z \mid X).
$$


<!-- <p>&nbsp;</p> -->

<h5>
  <span class="header-underline">GIBBS' INEQUALITY</span>
</h5>

For a random variable $$X$$, consider two probability distributions $$P$$ and $$Q.$$ Gibbs' inequality states that the entropy of $$X$$ according to $$P$$ is smaller than or equal to the cross-entropy of $$P$$ and $$Q:$$

$$
\begin{aligned}
H(P, P) &\leq H(P, Q), \\
-\sum_x p(x) \log p(x) &\leq -\sum_x p(x) \log q(x).
\end{aligned}
$$

See this [Wikipedia page](https://en.wikipedia.org/wiki/Gibbs%27_inequality) for the proof.


<!-- <p>&nbsp;</p> -->

<h5>
  <span class="header-underline">DATA-PROCESSING INEQUALITY</span>
</h5>

Given a Markov chain $$X \rightarrow Y \rightarrow g(Y)$$, where $$g$$ is a data processing function on $$Y$$, then $$I(X;Y) \geq I\left(X; g(Y)\right)$$.

> *Proof.* Here we have two ways to expand the mutual information using chain rule:
>
> $$
> \begin{aligned}
> I\left(X; Y, g(Y)\right) &= I(X; g(Y)) + I(X; Y \mid g(Y))\\
> &= I(X; Y) + I(X; g(Y) \mid Y).
> \end{aligned}
> $$
> 
> Since $$X \perp g(Y) \mid Y$$ as a property of Markov chain, $$I(X; g(Y) \mid Y) = 0$$. Thus
> 
> $$
> I(X; g(Y)) + I(X; Y \mid g(Y)) = I(X; Y).
> $$
> 
> Then, since $$I(X; Y \mid g(Y)) \geq 0$$, we can prove that $$I(X; Y) \geq I(X; g(Y)).$$ <span style="float: right;">$$\blacksquare$$</span>
{: .shorter}

<!-- <p markdown="1" style="text-align: right;">$$\blacksquare$$</p> -->

The data-processing inequality indicates that any data processing function on $$Y$$ cannot increase its mutual information with $$X.$$ Be careful though that this inequality is only guaranteed to hold when $$X$$, $$Y$$, and $$g(Y)$$ form a Markov chain.


<p>&nbsp;</p>

<span class="back-link">
  <i class="fa-solid fa-arrow-left"></i>
  <a href="{{ '/resources/' | relative_url }}">Back to Resources</a>
</span>

---
[^true-dist]: The true distribution is typically on the left in terms of notation.
[^binary-entropy]: $$H(p_e) = -p_e \log p_e - (1-p_e) \log (1 - p_e)$$


