---
layout: page
permalink: /resources/predictive-coding/
title: Variational Free Energy and Predictive Coding
description: 
nav: false
---

<div class="page-note">
  <p>These are my personal study notes based on:</p>
  <ul>
    <li><a href="https://doi.org/10.1016/j.jml.2025.104705"><em>A predictive coding model for online sentence processing</em></a> (Ohams et al., 2026)</li>
    <li><a href="https://doi.org/10.1016/j.jmp.2017.09.004"><em>The free energy principle for action and perception: A mathematical review</em></a> (Buckley et al., 2017)</li>
    <li><a href="https://doi.org/10.1371/journal.pcbi.1011183"><em>Predictive coding networks for temporal prediction</em></a> (Millidge et al., 2024)</li>
  </ul>
  <p>All errors are my own. Please refer to the original texts for complete and authoritative treatment.</p>
</div>


<span class="back-link">
  <i class="fa-solid fa-arrow-left"></i>
  <a href="{{ '/resources/' | relative_url }}">Back to Resources</a>
</span>


---

<br/>


<h5 id="variational-free-energy">
  <span class="header-underline">VARIATIONAL FREE ENERGY</span>
</h5>

<p class="page-note">This section follows <a href="https://doi.org/10.1016/j.jmp.2017.09.004">(Buckley et al., 2017)</a></p>

<!-- This section follows [Buckley et al. (2017)](https://doi.org/10.1016/j.jmp.2017.09.004). -->




<span class="subsection-header">Setting and Motivation</span>


**Generative model $$\vartheta \rightarrow \varphi$$.** &nbsp; In this generative model, the sensory observation $$\varphi$$ is generated from the environmental state $$\vartheta.$$

**Issue with Bayesian inference about $$\vartheta$$.** &nbsp; To infer $$\vartheta$$ given $$\varphi$$, we use Bayes' rule

$$
p \left( \vartheta \mid \varphi = \phi \right) = \frac{p\left( \phi \mid \vartheta \right) p(\vartheta)}{p(\varphi = \phi)} = \frac{p\left( \phi \mid \vartheta \right) p(\vartheta)}{\int p\left( \phi \mid \vartheta \right) p(\vartheta) \, d\vartheta}.
$$

However, the marginalization in the denominator is often intractable, so we want to approximate this posterior distribution $$p \left( \vartheta \mid \varphi \right).$$ We do this by introducing an auxiliary probability density $$q(\vartheta)$$, and we want to use $$q(\vartheta)$$ to approximate $$p \left( \vartheta \mid \varphi \right).$$



<span class="subsection-header">Evaluating the Approximation</span>


We can use KL divergence to evaluate how good the approximation is:

$$
D_{\mathrm{KL}} \left[ q(\vartheta) \parallel p \left( \vartheta \mid \varphi \right) \right]
= \int q(\vartheta) \ln \frac{q(\vartheta)}{p(\vartheta \mid \varphi)} \, d\vartheta.
$$

A good approximation should minimize this $$D_\mathrm{KL}.$$ But this is still not practical enough, since we don't really know $$p(\vartheta \mid \varphi).$$ So we rewrite $$D_\mathrm{KL}$$ as

$$
\begin{aligned}
D_{\mathrm{KL}} \left[ q(\vartheta) \parallel p \left( \vartheta \mid \varphi \right) \right]
&= \int q(\vartheta) \ln \frac{q(\vartheta)}{p(\vartheta \mid \varphi)} \, d\vartheta \\
&= \int q(\vartheta) \ln \frac{q(\vartheta)}{p(\vartheta, \varphi) / p(\varphi)} \, d\vartheta \\
&= \int q(\vartheta) \ln \frac{q(\vartheta) p(\varphi)}{p(\vartheta, \varphi)} \, d\vartheta \\
&= \underbrace{\int q(\vartheta) \ln \frac{q(\vartheta)}{p(\vartheta, \varphi)} \, d\vartheta}_{\text{free energy } \mathcal{F}} \, + \, \int q(\vartheta) \ln p(\varphi) \, d\vartheta \\
&= \mathcal{F} \, + \, \ln p(\varphi).
\end{aligned}
$$

So the free energy $$\mathcal{F}$$ is

$$
\begin{aligned}
\mathcal{F} &= D_{\mathrm{KL}} - \ln p(\varphi) \\
&= \underbrace{D_{\mathrm{KL}}}_{\geq 0} \, + \, \text{surprisal of sensory input } \varphi.
\end{aligned}
$$

Since the sensory input $$\varphi$$ is not something we can control, at least not in this naive generative model, minimizing $$\mathcal{F}$$ is equivalent to minimizing $$D_{\mathrm{KL}}.$$ Also, since $$D_{\mathrm{KL}}$$ is non-negative, $$\mathcal{F}$$ is an upper bound of surprisal. Therefore, our objective is now to find a $$q(\vartheta)$$ that minimizes $$\mathcal{F}.$$



<span class="subsection-header">Connection to Predictive Coding</span>


To derive the connection to predictive coding, we can first rewrite free energy $$\mathcal{F}$$ as

$$
\begin{aligned}
\mathcal{F} &= \int q(\vartheta) \ln \frac{q(\vartheta)}{p(\vartheta, \varphi)} \, d\vartheta \\
&= \int q(\vartheta) \ln q(\vartheta) \, d\vartheta \, + \, \int q(\vartheta) \underbrace{\ln \frac{1}{p(\vartheta, \varphi)}}_{E(\vartheta, \varphi)} \, d\vartheta.
\end{aligned}
$$

Let's define $$E(\vartheta, \varphi) = -\ln p(\vartheta, \varphi)$$, yielding

$$
\mathcal{F} = \int q(\vartheta) \ln q(\vartheta) \, d\vartheta \, + \, \int q(\vartheta) E(\vartheta, \varphi) \, d\vartheta.
$$

Now, to establish the connection with predictive coding, we need to make some assumptions to further simplify free energy $$\mathcal{F}.$$

> **Assumption 1: $$q(\vartheta)$$ takes the Gaussian form.**
> 
> $$
> \begin{aligned}
> q(\vartheta) &= \mathcal{N}\left(\vartheta; \mu, \sigma^2\right) \\
> &= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left\{ -\frac{(\vartheta - \mu)^2}{2 \sigma^2} \right\} \\
> &= \frac{1}{Z}e^{-\varepsilon (\vartheta)} \qquad \text{letting } Z=\sqrt{2 \pi \sigma^2}, \; \varepsilon (\vartheta)= \frac{(\vartheta - \mu)^2}{2 \sigma^2}.
> \end{aligned}
> $$
{: .shorter}

We can expand free energy $$\mathcal{F}$$ accordingly:

$$
\begin{aligned}
\mathcal{F} &= \int q(\vartheta) \ln \left(\frac{1}{Z} e^{-\varepsilon(\vartheta)} \right) d\vartheta \, + \int q(\vartheta) E(\vartheta, \varphi) \, d\vartheta \\
&= \int q(\vartheta) \left( -\ln Z - \varepsilon(\vartheta) \right) d\vartheta + \int q(\vartheta) E(\vartheta, \varphi) \, d\vartheta \\
&= -\int q(\vartheta) \ln Z \, d\vartheta - \int q(\vartheta) \varepsilon(\vartheta) \, d\vartheta + \int q(\vartheta) E(\vartheta, \varphi) \, d\vartheta \\
&= -\underbrace{\vphantom{ \int } \ln Z}_{\text{①}} \, - \underbrace{\int q(\vartheta) \varepsilon(\vartheta) \, d\vartheta}_{\text{②}} \, + \underbrace{\int q(\vartheta) E(\vartheta, \varphi) \, d\vartheta }_{\text{③}}.
\end{aligned}
$$

For ① and ②:

$$
\text{①} = \ln Z = \frac{1}{2} \ln 2 \pi \sigma^2;
$$

$$
\begin{aligned}
\text{②} &= \int q(\vartheta) \frac{(\vartheta - \mu)^2}{2\sigma^2} \, d\vartheta \\
&= \frac{1}{2 \sigma^2} \underbrace{\int q(\vartheta) (\vartheta - \mu)^2 \, d\vartheta}_{\mathbb{E}_\vartheta \left[(\vartheta - \mu)^2\right] = \sigma^2} = \frac{1}{2}.
\end{aligned}
$$

For ③, we can simplify it by making another assumption as below.

> **Assumption 2: $$q(\vartheta)$$ is tightly peaked, like a delta function.** We assume that $$q(\vartheta)$$ is a tightly peaked Gaussian, where its density is concentrated at its mean $$\mu.$$ That is, $$q(\vartheta)$$ is non-zero only around $$\vartheta = \mu$$, within a small increment $$\delta \vartheta = \vartheta - \mu.$$ As a result, the integrand in ③ is only non-zero within the small region around $$\vartheta = \mu.$$
{: .shorter}

<figure style="text-align: center;">
  <img class="plot-figure" src="{{ '/assets/img/information-theory/peaked-gaussian.svg' | relative_url }}" alt="A tightly peaked Gaussian q(theta) centered at mu" style="max-width: 420px; width: 100%;">
  <figcaption class="page-note">\(q(\vartheta)\) as a tightly peaked Gaussian centered at \(\mu\)</figcaption>
</figure>

Now, for ③, since $$q(\vartheta) E(\vartheta, \varphi)$$ is only non-zero near $$\vartheta = \mu$$, to take the integral, which is the area under $$q(\vartheta) E(\vartheta, \varphi)$$, we only need to consider that small region near $$\mu.$$ Therefore, we can approximate $$E(\vartheta, \varphi)$$ through a Taylor expansion:

$$
\begin{aligned}
\text{③} &= \int q(\vartheta) E(\vartheta, \varphi) \, d\vartheta \\
&\approx \int q(\vartheta) \left\{ E(\mu, \varphi) + \left[ \frac{dE}{d\vartheta} \right]_\mu \delta \vartheta + \frac{1}{2} \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu \delta \vartheta^2 \right\} d \vartheta \\
&= \int q(\vartheta) E(\mu, \varphi) \, d\vartheta \, + \int q(\vartheta) \left[ \frac{dE}{d\vartheta} \right]_\mu (\vartheta - \mu) \, d\vartheta \, + \int \frac{1}{2} q(\vartheta) \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu (\vartheta - \mu)^2 \, d\vartheta \\
&= E(\mu, \varphi) \, + \, \xcancel{ \left[ \frac{dE}{d\vartheta} \right]_\mu \underbrace{\int q(\vartheta) (\vartheta - \mu) \, d\vartheta}_{\mathbb{E}_\vartheta \left[\vartheta - \mu\right] = 0} } \, + \, \frac{1}{2} \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu \underbrace{\int q(\vartheta) (\vartheta - \mu)^2 \, d\vartheta}_{\mathbb{E}_\vartheta \left[(\vartheta - \mu)^2\right] = \sigma^2} \\
&= E(\mu, \varphi) + \frac{1}{2} \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu \sigma^2.
\end{aligned}
$$

Therefore, given the first two assumptions, we've simplified free energy $$\mathcal{F}$$ to

$$
\begin{aligned}
\mathcal{F} &= -\text{①} - \text{②} + \text{③} \\
&= E(\mu, \varphi) + \frac{1}{2} \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu \sigma^2 - \frac{1}{2} - \frac{1}{2} \ln 2 \pi \sigma^2.
\end{aligned} \tag{1}
$$

> Note that instead of dealing with the whole distribution of $$\vartheta$$, we are now only dealing with its mean $$\mu.$$ And instead of $$E(\vartheta, \varphi)$$, we are now dealing with $$E(\mu, \varphi)$$, which is termed the *Laplace-encoded energy*.
{: .shorter .page-note}

We can further simplify $$\mathcal{F}$$ by getting rid of the dependency on the variance $$\sigma^2.$$ To minimize $$\mathcal{F}$$ w.r.t. $$\sigma^2$$, we take the derivative and set it to 0:

$$
\frac{d\mathcal{F}}{d\sigma^2} = \frac{1}{2} \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu - \frac{1}{2} \frac{1}{2 \pi \sigma^2} 2\pi = \frac{1}{2} \left( \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu - \frac{1}{\sigma^2} \right) = 0.
$$

Thus, the optimal variance $$\sigma^{2*}$$ that minimizes $$\mathcal{F}$$ is a function of the mean $$\mu$$:

$$
\sigma^{2*} = \left[ \frac{d^2 E}{d\vartheta^2} \right]_\mu^{-1}. \tag{2}
$$

Plugging Eq. (2) back into Eq. (1) yields

$$
\mathcal{F} = E(\mu, \varphi) - \frac{1}{2} \ln (2 \pi \sigma^{2*}). \tag{3}
$$

Here's what I didn't fully follow, but it turns out that we can get rid of the second term in Eq. (3) and only focus on $$E(\mu, \varphi).$$ That is, to minimize $$\mathcal{F}$$, our new objective is to find the optimal $$\mu^*$$ that minimizes the Laplace-encoded energy $$E(\mu, \varphi)$$:

$$
\mu^* = \operatorname*{arg\,min}_\mu E(\mu, \varphi) = \operatorname*{arg\,min}_\mu \left[ -\ln p(\mu, \varphi) \right].
$$

Now we make the last assumption to derive $$p(\mu, \varphi).$$

> **Assumption 3: $$\mu$$ and $$\varphi$$ are drawn from Gaussians.** In order to figure out $$p(\mu, \varphi)$$, let's now define a generative model, where both $$\mu$$ and $$\varphi$$ are assumed to be drawn from Gaussian distributions (recall that $$\mu$$ is the center of the distribution for the environmental state $$\vartheta$$):
{: .shorter}

$$
\begin{aligned}
\mu &= \mu_0 + \eta_\mu \qquad \text{where } \eta_\mu \sim \mathcal{N}(0, \sigma^2_{\mu}); \\
\varphi &= g(\mu; \theta) + \eta_\varphi \qquad \text{where } \eta_\varphi \sim \mathcal{N}(0, \sigma^2_{\varphi}).
\end{aligned}
$$

In this generative process, $$\mu$$ is drawn from a prior guess $$\mu_0$$ with some Gaussian noise $$\eta_\mu.$$ Similarly, the sensory observation is generated from $$\mu$$ through some parameterized function $$g_\theta$$ with Gaussian noise $$\eta_\varphi.$$ For example, you can imagine $$g$$ as a simple linear model with weights $$\theta$$ (e.g., $$\varphi = \theta \mu + \eta_\varphi$$).

Given these Gaussian assumptions and the generative model, we have

$$
\begin{aligned}
p(\mu, \varphi) &= p(\varphi \mid \mu) p(\mu); \\
p(\mu) &= \frac{1}{\sqrt{2 \pi \sigma^2_\mu}} e^{-\frac{(\mu - \mu_0)^2}{2\sigma^2_\mu}}; \\
p(\varphi \mid \mu) &= \frac{1}{\sqrt{2 \pi \sigma^2_\varphi}} e^{-\frac{(\varphi - g(\mu; \theta))^2}{2\sigma^2_\varphi}}.
\end{aligned}
$$

Recall that we want to minimize $$E(\mu, \varphi).$$ So plugging these terms into $$E$$ yields

$$
\begin{aligned}
E(\mu, \varphi) &= -\ln p(\mu, \varphi) \\
&= -\ln p(\varphi \mid \mu) - \ln p(\mu) \\
&= -\ln \left( \frac{1}{\sqrt{2 \pi \sigma^2_\varphi}} e^{-\frac{\left(\varphi - g(\mu; \theta) \right)^2}{2\sigma^2_\varphi}} \right) - \ln \left( \frac{1}{\sqrt{2 \pi \sigma^2_\mu}} e^{-\frac{\left(\mu - \mu_0 \right)^2}{2\sigma^2_\mu}} \right) \\
&= \ln \sqrt{2 \pi \sigma^2_\varphi} + \frac{\left(\varphi - g(\mu; \theta) \right)^2}{2\sigma^2_\varphi} + \ln \sqrt{2 \pi \sigma^2_\mu} + \frac{(\mu - \mu_0)^2}{2\sigma^2_\mu} \\
&= \frac{1}{2 \sigma^2_\varphi} \Big( \underbrace{\varphi - g(\mu; \theta)}_{\substack{\varepsilon_{\varphi} \\ \text{sensory pred. error}}} \Big)^2 + \frac{1}{2 \sigma^2_\mu} \Big( \underbrace{\mu - \mu_0}_{\substack{\varepsilon_{\mu} \\ \text{state pred. error}}} \Big)^2 + \frac{1}{2}\ln \sigma^2_\varphi \sigma^2_\mu + \ln 2\pi \\
&= \frac{1}{2 \sigma^2_\varphi} \varepsilon_{\varphi}^2 + \frac{1}{2 \sigma^2_\mu} \varepsilon_{\mu}^2 + \frac{1}{2}\ln \sigma^2_\varphi \sigma^2_\mu + C.
\end{aligned} \tag{4}
$$

So the variational free energy depends on two prediction errors, the sensory prediction error $$\varepsilon_{\varphi}$$ and the internal-state prediction error $$\varepsilon_{\mu}$$, weighted by their precisions $$1/\sigma^2_{\varphi}$$ and $$1/\sigma^2_{\mu}.$$

The full multivariate version of Eq. (4) would be Eq. (14) in Ohams et al. (2026).






<br/>

<h5 id="temporal-predictive-coding">
  <span class="header-underline">TEMPORAL PREDICTIVE CODING</span>
</h5>

<p class="page-note">This section follows <a href="https://doi.org/10.1371/journal.pcbi.1011183">Millidge et al. (2024)</a></p>

<!-- This section follows [Millidge et al. (2024)](https://doi.org/10.1371/journal.pcbi.1011183). -->

Millidge et al. (2024) use different notation from Buckley et al. (2017). In this section, sensory observations are $$y$$, and the internal states are $$x.$$ Moreover, Millidge et al. (2024) use the notation of state-space representation, but I'll just work with the notation in Ohams et al. (2026). Again, I will work with the univariate version for derivation.

As derived in the basic predictive coding model in the [Variational Free Energy](#variational-free-energy) section, our objective function is

$$
\mathcal{F}_k = \frac{1}{2 \sigma_y^2} \varepsilon_{y_k}^2 + \frac{1}{2 \sigma_x^2} \varepsilon_{x_k}^2 + \frac{1}{2} \ln \sigma_y^2 \sigma_x^2,
$$

where the prediction errors are

$$
\varepsilon_{y_k} = y_k - f(x_k; \theta_y); \qquad \varepsilon_{x_k} = x_k - f(\hat{x}_{k-1}; \theta_x),
$$

$$
f(x_k; \theta_y) = \theta_{yw}g(x_k) + \theta_{yb}; \qquad f(\hat{x}_{k-1}; \theta_x) = \theta_{xw}g(\hat{x}_{k-1}) + \theta_{xb}.
$$

The only difference from the previous section is that the state prediction error $$\varepsilon_{x_k}$$ is now defined for each time step $$k$$ at which each linguistic input is received. It is the difference between the inferred state $$x_k$$ and the predicted state $$f(\hat{x}_{k-1}; \theta_x)$$, where $$f$$ is some kind of linear function parameterized by $$\theta$$ that gives rise to a prediction. The function $$g$$ simply captures any potential non-linearity, and $$\theta_b$$ is the bias term of the linear function.



<span class="subsection-header">Inferring the State $$x_k$$</span>


We can infer $$x_k$$ through gradient descent on $$\mathcal{F}.$$ The update of $$x_k$$ at each computation time is based on the derivative:

$$
\frac{d}{dt} x_k = -\frac{\partial \mathcal{F}_k}{\partial x_k}.
$$

Note that the time index $$t$$ is different from the time index $$k.$$ The discrete time index $$k$$ is the time at which the observations $$y$$ arise, and the continuous real time $$t$$ is the time in which computations are made within the model (I assume this $$t$$ can be considered the number of iterations needed for $$x_k$$ to arrive at a converged solution).

Let's first take a closer look at $$\mathcal{F}.$$ If we want to take its derivative w.r.t. $$x_k$$, then

$$
\mathcal{F}_k = \underbrace{\frac{1}{2 \sigma_y^2} \varepsilon_{y_k}^2}_{\substack{\varepsilon_y \text{ is a function of } x_k\text{,}\\ \text{so keep this term}}} + \underbrace{\frac{1}{2 \sigma_x^2} \varepsilon_{x_k}^2}_{\substack{\varepsilon_x \text{ is a function of } x_k\text{,}\\ \text{so keep this term too}}} + \underbrace{\frac{1}{2} \ln \sigma_y^2 \sigma_x^2}_{\substack{\text{not a function of } x_k\text{,}\\ \text{so drop it for derivative}}}
$$

Therefore,

$$
\begin{aligned}
\frac{d}{dt} x_k &= -\frac{\partial \mathcal{F}_k}{\partial x_k} \\
&= -\left( \frac{1}{2 \sigma_y^2} 2 \varepsilon_{y_k} \frac{\partial \varepsilon_{y_k}}{\partial x_k} + \frac{1}{2\sigma_x^2} 2 \varepsilon_{x_k} \frac{\partial \varepsilon_{x_k}}{\partial x_k} \right) \qquad \text{(chain rule)} \\
&= -\frac{\varepsilon_{y_k}}{\sigma_y^2} \underbrace{\frac{\partial \varepsilon_{y_k}}{\partial x_k}}_{\text{④}} - \frac{\varepsilon_{x_k}}{\sigma_x^2} \underbrace{\frac{\partial \varepsilon_{x_k}}{\partial x_k}}_{\text{⑤}}
\end{aligned} \tag{5}
$$

Now, let's take a look at the derivatives of $$\varepsilon_{y_k}$$ and $$\varepsilon_{x_k}.$$

For $$\text{④} = \frac{\partial \varepsilon_{y_k}}{\partial x_k}$$, recall that $$\varepsilon_{y_k} = y_k - f(x_k; \theta_y).$$ The function $$f$$ is some form of linear function, as repeated below

$$
f(x_k; \theta_y) = \theta_{yw}g(x_k) + \theta_{yb},
$$

where $$g$$ captures any potential non-linearity and $$\theta_{yb}$$ is the bias term. Therefore,

$$
\frac{\partial \varepsilon_{y_k}}{\partial x_k} = \frac{\partial}{\partial x_k} \left(y_k - f(x_k; \theta_y)\right) = - \frac{\partial f(x_k; \theta_y)}{\partial x_k}. \tag{6}
$$

For $$\text{⑤} = \frac{\partial \varepsilon_{x_k}}{\partial x_k}$$, the derivative is much simpler. Recall that $$\varepsilon_{x_k} = x_k - f(\hat{x}_{k-1}; \theta_x)$$, where $$f$$ is not a function of $$x_k.$$ Therefore,

$$
\frac{\partial \varepsilon_{x_k}}{\partial x_k} = \frac{\partial}{\partial x_k} \left( x_k - f(\hat{x}_{k-1}; \theta_x) \right) = 1. \tag{7}
$$

Plugging Eq. (6) and Eq. (7) back into Eq. (5) yields

$$
\frac{d}{dt} x_k = - \frac{\varepsilon_{x_k}}{\sigma_x^2} + \frac{\varepsilon_{y_k}}{\sigma_y^2} \frac{\partial f(x_k; \theta_y)}{\partial x_k}.
$$

The full multivariate version of this would be Eq. (22) in [Ohams et al. (2026)](https://doi.org/10.1016/j.jml.2025.104705), where $$\sigma^2$$ is substituted with the covariance matrix $$\Sigma.$$



<span class="subsection-header">Learning Parameters $$\theta$$</span>


After inferring $$x_k$$, they do a one-step update for the parameters $$\theta$$ through gradient descent, given by Eq. (8), where $$\eta$$ is the learning rate. The derivation for each of these derivatives is similar to the one above for inferring the state $$x_k.$$

$$
\Delta \theta_{yw} = -\eta\frac{\partial \mathcal{F}_k}{\partial \theta_{yw}}; \qquad \Delta \theta_{xw} = -\eta\frac{\partial \mathcal{F}_k}{\partial \theta_{xw}}; \qquad \Delta \theta_{yb} = -\eta\frac{\partial \mathcal{F}_k}{\partial \theta_{yb}}; \qquad \Delta \theta_{xb} = -\eta\frac{\partial \mathcal{F}_k}{\partial \theta_{xb}}. \tag{8}
$$

Millidge et al. (2024) claim that the $$\theta$$s are updated in only one step because these variables are assumed to represent more slowly changing variables in the real world, and are assumed to be implemented by synaptic strengths, which typically change slowly. In contrast, the internal state $$x_k$$ corresponds to neural firing rates, which change quickly.

Let's work through $$\Delta \theta_{yw}$$ and $$\Delta \theta_{xw}$$, and you will get the idea. The objective $$\mathcal{F}_k$$ is repeated below:

$$
\mathcal{F}_k = \frac{1}{2 \sigma_y^2} \varepsilon_{y_k}^2 + \frac{1}{2 \sigma_x^2} \varepsilon_{x_k}^2 + \frac{1}{2} \ln \sigma_y^2 \sigma_x^2;
$$

$$
\varepsilon_{y_k} = y_k - f(x_k; \theta_y); \qquad \varepsilon_{x_k} = x_k - f(\hat{x}_{k-1}; \theta_x);
$$

$$
f(x_k; \theta_y) = \theta_{yw}g(x_k) + \theta_{yb}; \qquad f(\hat{x}_{k-1}; \theta_x) = \theta_{xw}g(\hat{x}_{k-1}) + \theta_{xb}.
$$

As a reminder, $$g$$ is simply a function that captures any potential non-linearity and does not contain $$\theta.$$

**For $$\Delta \theta_{yw}$$:**

$$
\begin{aligned}
\Delta \theta_{yw} &= -\eta\frac{\partial \mathcal{F}_k}{\partial \theta_{yw}}\\
&= -\eta \frac{1}{2 \sigma_y^2} 2 \varepsilon_{y_k} \frac{\partial \varepsilon_{y_k}}{\partial \theta_{yw}} \qquad \text{(chain rule)} \\
&= -\eta \frac{\varepsilon_{y_k}}{\sigma_y^2} \left( - \frac{\partial f(x_k; \theta_y)}{\partial \theta_{yw}} \right) \qquad \text{(chain rule)} \\
&= -\eta \frac{\varepsilon_{y_k}}{\sigma_y^2} (-g(x_k)) \\
&= \eta \frac{\varepsilon_{y_k}}{\sigma_y^2} g(x_k).
\end{aligned}
$$

The multivariate version of this would be Eq. (26) in [Ohams et al. (2026)](https://doi.org/10.1016/j.jml.2025.104705), where the variance $$\sigma_y^2$$ is substituted with the covariance matrix $$\Sigma_y.$$

**For $$\Delta \theta_{xw}$$:**

$$
\begin{aligned}
\Delta \theta_{xw} &= -\eta\frac{\partial \mathcal{F}_k}{\partial \theta_{xw}}\\
&= -\eta \frac{\varepsilon_{x_k}}{\sigma_x^2} \left( - \frac{\partial f(\hat{x}_{k-1}; \theta_x)}{\partial \theta_{xw}} \right) \qquad \text{(chain rule)} \\
&= \eta \frac{\varepsilon_{x_k}}{\sigma_x^2} g(\hat{x}_{k-1}).
\end{aligned}
$$

The multivariate version of this would be Eq. (27) in [Ohams et al. (2026)](https://doi.org/10.1016/j.jml.2025.104705), where the variance $$\sigma_x^2$$ is substituted with the covariance matrix $$\Sigma_x.$$


<p>&nbsp;</p>

---

<span class="back-link">
  <i class="fa-solid fa-arrow-left"></i>
  <a href="{{ '/resources/' | relative_url }}">Back to Resources</a>
</span>
