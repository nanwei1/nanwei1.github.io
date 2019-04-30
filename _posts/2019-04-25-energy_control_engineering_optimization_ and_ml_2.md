---
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true

title:  "Energy, Control Engineering, Optimization, and ML - II"
---

My last post discussed control engineering at a rather high level. In this post, I will include a bit more technical stuff in order to illustrate a comparison with optimization and ML.

## Energy
Many control problems can be defined in the following generic form: given states $x$ (which could be and often is a vector), control input $u$, and known system dynamics $\dot{x} = f\( x, u \)$, find a choice of $u$ to make $x$ converge to some desired states $x_d$.

Using the pendulum example from my previous post again, the state $x$ can be chosen to be the orientation of the pendulum along with the angular velocity $x = \( \theta, \dot\theta \)$. Why do we need the velocity term? Then we have $\dot x = \( \dot\theta, \textrm{some equation}\)$, where *some equation* is the equation of motion for $\ddot \theta$ as a function of $m$, $\theta$, $l$, and $u$. In this case, $u=\tau$ is the input torque at the pivot. If the goal is to stabilize the pendulum at certain orientation $\theta_d$, then we have $x_d = \( \theta_d, 0\)$.

The essence of the energy-based control that was discussed in my last post is to understand the natural energy that is present in the system, identify ways to "replace" it with an artificial form that favors the target $x_d$ as the globally stable equilibrium point. For a fully actuated system (meaning that you can exert input independently in the direction of each degree of freedom), it is often very easy to find $u$ once the artificial energy is well-defined, so the challenge is usually find the artificial energy itself. Because the artificial energy is usually taken as the Lyapunov function, we denote is as $V(x, \dot x)$. Loosely speaking, if you can prove that $V$ is at its minimum at $(x=x_d, \dot x=0)$, and that $V$ monotonically decreases over time (*i.e.* $\dot V(x, \dot x, u) <0)$, then we know that the desired states will eventually be reached.


## Optimization and ML
Of course there is no Lyapunov function in optimization, but instead we have a scalar function that defines the cost to be minimized: $J(x,y,\theta)$. Here $x$ is the input data, $y$ is the output data, and $\theta$ is the model parameters (weights and biases in the neural network, for instance). My apologies for mixing the use of $x$ and $\theta$, but I was following the most widely used conventions in both controls and optimization so please bear with me here.

In most cases, it is impossible to get a closed-form solution to the optimization problem, so we have to figure out other ways to decrease $J$. The most widely used method is an iterative one, often based on the gradient - backpropagation in neural networks is a great example of this. In each iteration, the goal is to gradually decrease $J$ by adjusting $\theta$ - *i.e.* we aim to find some update rule $\theta_{i+1} \leftarrow g(\theta_i, \frac{\partial J}{\partial \theta_i})$ where $i$ denotes the time step or number of iterations, so that $J_{i+1} < J_i$. To make it \*a bit\* more concrete, the vanilla gradient descent rule has $\theta_{i+1} \leftarrow \theta_i - \alpha \frac{\partial J}{\partial \theta_i}$, and with a suitable choice of $\alpha$ we know the cost will decrease after each iteration.

I personally found this very similar to energy-based control. In the end, in both cases the goal is to minimize certain scalar function $V$ or $J$. There is no direct notion of "time derivative" in optimization, but the update rule is no different from a discrete system in control engineering. If we make the update step small enough (very small $\alpha$ in gradient descent, for example), it can be perceived as a continuous time control system. This goes along with the analogy that gradient descent is similar to a ball rolling downhill in a complex high-dimensional space (I actually have some slight disagreement about this analogy and will discuss this in a future post).

## Comparison Chart

|   |  Control Engineering | ML Optimization  | Notes  |
|:---:|:---:|:---:|:---:|
|  Scalar Function | Lyapunov function $V$  | Cost function $J$  | Usually $J$ is given for an optimization problem, but finding a suitable $V$ is often the hardest step in control |
| "Space"  | State space of $x$  |  Space of all possible model parameters $\theta$ |   |   
| Initial Condition |  Usually constrained by initial condition $x(t=0) = x_0$ | Free to initialize $\theta$ |   |
|Objective | Make $x \rightarrow x_d$ (target configuration). In doing so we minimize $V$ | Minimize cost $J$. In doing so we reach some optimal parameters $\theta_d$ | The target configuration $x_d$ is always given in control, but we never know $\theta_d$ ahead of time - otherwise there's no point working on optimization! |
| "Input" | Input is $u$ which goes in the system dynamics: $\dot x = f(u, x)$. We are constrained by this $f(u, x)$ which defines how the input affects the system | The "input" is the update rule itself, free of constraints: $\theta_{i+1} \leftarrow g(\theta_i, \frac{\partial J}{\partial \theta_i})$| |
| "Dynamics" | Dynamics of the closed-loop system - "energy always tends to decrease" | Gradient-based optimization - "rolling downhill" *aka* "energy always tends to decrease" | |

Looking back at this post I almost feel that this is a rambling note and there should be better ways to tell the story. If you have any questions or suggestions please feel out to reach out!
