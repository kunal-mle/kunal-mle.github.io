---
layout: post
title:  "Optimizers"
date:   2022-03-03 7:37:38 -0800
categories: topics
---

## Momentum

We maintain the running mean of the gradients, which then updates the parameters. The idea here is to trust the past gradients a little when making a new step instead of relying entirely on the current gradient only. There are two advantages of this, one direct and one indirect, respectively listed below:
- In methods like stochastic gradient descent, it helps take completely wrong steps due to noisy gradients, because while it considers the current gradient, it adds it to the past accumulated gradients as well - which more or less would point towards the correct direction. Think of it like this: imagine you are trying to reach a place in a new city solely based on taking directions from strangers that you meet on the way. Normally, most of the people that you ask are going to point you in the right direction, while some people may point you in a completely wrong direction. So in such a case, ideally you would be more confident about the direction that the past 100 people gave you instead of blindly following a new direction given by the 101th person. But you don't want to discard this new direction completely either, you are not sure if it's really wrong. So you start walking in a direction somewhat in between the past directions and this new direction. That's how momentum helps with noisy directions(gradients).
- It could help avoid local minima that are saddle points by not reducing *speed* entirely when it comes across one and rather continuing to move past it - because even though the gradient is close to 0 at this local minima, there is still the component of the past gradients which would make you keep moving (similar to the concept of inertia in physics). Note that although this can be seen as an advantage in such a scenario, it could also work as a disadvantage by escaping even a good local optimum.

Here is the algorithm for gradient descent using momentum optimization:
- Initialize $$v=0$$, set $$\alpha\in[0,1]$$: typically 0.9 or 0.99
- Then repeat the following until stopping criteria is met:
    - Compute gradient: $$g$$
    - Update: $$v\leftarrow(\alpha v-\epsilon g)$$
    - Gradient step: $$\theta\leftarrow (\theta+v)$$

---

## Annealing and Learning Rate Schedules

While momentum proved to work better than the vanilla gradient descent, there needs to be some way of solving the problem of it escaping the local minima. The way momentum made convergence faster was to give weightage to the past (averaged) gradients as well - thereby eliminating unecessary traversal in wrong directions due to noisy gradients thereby saving time.

Another good approach would be to start with a big learning rate initially for faster learning and then reduce it once the loss function doesn't change much (due to zig zagging because of big learning rate). 

{:refdef: style="text-align: center;"}
![img0](https://www.dropbox.com/s/wlduk14w7vygor2/Screenshot%202022-03-03%20at%208.35.33%20AM.png?raw=1){: width="75%" }
{: refdef}

This is called 'Annealing' and as simple it looks, it is in fact practically used some times even today. The only downside is that it needs manual intervention and continuous observation. So while it can be tried as an additional measure once in a while, this is not something we can always rely on solely due to the human effort it requires.

We need to come up with a way to automatically lower the learning rate over time. One way is to *schedule* this decay and this is called Learning Rate Schedule. There are different ways to do this including time-based decay, step decay and exponential decay.
- Time based decay: it reduces the rate by a factor inversely proportional to the iteration number
- Step decay: it reduces the rate by a constant factor every few epochs, ex: reduce by half every 10 epochs
- Exponential decay: it reduces the rate by a factor exponentially proportional to the iteration number

While these methods work well, we can do better than following a fixed decay rule everytime. Enter the adaptive gradient methods - which vary the learning rate adaptively based on the past gradients.

---

## Adaptive Gradient Method (AdaGrad)

The idea here is that we can reduce the laerning rate differently for different directions. Specifically, we want to reduce the learning rate more for those directions that have been pointed out by the past gradients already. Intuitively, this makes sense because we are assuming that we have travelled large steps already using those directions that we encountered in the past gradients, hence it is better that we start slowing down slowly in those directions. This is done by dividing (element wise) the rate by a vector that keeps a historical sum of norm of all the past gradients. So, more this historical sum points towards a particular direction(dimension), more is the learning rate reduced for those directions.

Here is the algorithm for Adagrad:
- Initialize $$a=0$$. Set $$\nu$$ at a small value to avoid division by zero.
- Then repeat the following until stopping criteria is met:
    - Compute gradient: $$g$$
    - Update: $$a\leftarrow (a+g\odot g)$$
    - Gradient step: $$\theta\leftarrow\left(\theta - \dfrac{\epsilon}{\sqrt{a}+\nu}\odot g\right)$$

---

## RMSprop

One disadvantage of Adagrad is that once we stepped in certain direction, doesn't matter how long ago it was, we are *always* going to reduce the step size in that direction forever. This sometimes could cause too slow approach to the optima towards the end caused due to some noisy gradient in the right direction in the beginning.

To overcome this, we could come up with an averaging method of the historical gradients that gives less weightage to gradients which are older. This can be done via a running average of the past gradients instead of simply accumulating. This simple change done on top of the Adagrad is called RMSprop method.

Here is the algorithm for RMSprop:
- Initialize $$a=0$$. Set $$\nu$$ at a small value to avoid division by zero.
- Then repeat the following until stopping criteria is met:
    - Compute gradient: $$g$$
    - Update: $$a\leftarrow \left(\beta a+(1-\beta)g\odot g\right)$$
    - Gradient step: $$\theta\leftarrow\left(\theta - \dfrac{\epsilon}{\sqrt{a}+\nu}\odot g\right)$$

---

## Adam

We saw two different approaches of optimization - one via the Momentum method, another via the RMSprop method. While both are different from each other, what if we combine both of these? That's Adam or Adaptive Moment method for you. Just look at the algorithm below and you'll see how it combines velocity update(first moment) from Momentum and historical gradient update(second moment) from RMSprop.

Here is the algorithm for Adam:
- Initialize $$a=0$$. Set $$\nu$$ at a small value to avoid division by zero.
- Initialize $$v=0$$, set $$\alpha\in[0,1]$$: typically 0.9 or 0.99
- Then repeat the following until stopping criteria is met:
    - Compute gradient: $$g$$
    - First moment update: $$v\leftarrow(\beta_1 v-(1-\beta_1)g)$$
    - Second moment update: $$a\leftarrow \left(\beta_2 a+(1-\beta_2)g\odot g\right)$$
    - Gradient step: $$\theta\leftarrow\left(\theta - \dfrac{\epsilon}{\sqrt{a}+\nu}\odot v\right)$$

There is an improvement called *bias-correction* on top of this. Try to understand yourself about what could be the reasoning behind this (hint: it helps during the start of the journey):
- Calculate $$\tilde{v} = \left(\frac{1}{1-\beta_1^t}\right)v$$
- Calculate $$\tilde{a} = \left(\frac{1}{1-\beta_2^t}\right)a$$
- Use $$\tilde{v}$$ and $$\tilde{a}$$ instead of $$v$$ and $$a$$ to update $$\theta$$
