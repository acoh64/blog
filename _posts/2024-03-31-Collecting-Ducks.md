---
title: "Collecting Ducks"
date: 2025-03-31
---

One of the iconic parts of APS is the Physical Review journal ducks. 
During the career fair, APS has a spinning wheel that gives you one of 12 unique ducks, themed after different Physical Reviwe journals.

![Duck Wheel](/assets/images/duck_wheel.png)

On [their instagram page](https://www.instagram.com/aps.physics/reel/DHUmrSKiLde/), APS presented us with a challenge: “Can you collect ‘em all?”. 
And that is exactly what we sought to do. 
However, there were so many interesting talks going on at APS, so we couldn’t wait there for hours collecting ducks (or, more practically, that is not why our advisors paid for us to attend this conference). 
So we needed to know, how long would we expect to need to collect all of the ducks?

Since we are scientists, we went about solving this problem and collecting data to verify our results. While there are many ways to solve this classic problem (known as the [coupon collector’s problem](https://en.wikipedia.org/wiki/Coupon_collector%27s_problem)), we decided to use a fun method that, while not the simplest method, is general enough to answer many variants of the problem and provides an introduction to many interesting mathematical concepts. 

When you are collecting the ducks, there is just one essential thing that you need to keep track of: how many unique ducks have you collected. 
We will refer to this number as our state: if we are in state N, we have collected N unique ducks. 
The state we are in after X+1 spins only depends on the state we are in after X spins. In other words, your next state only depends on your current state. 
This type of system is known as a Markov chain.

Let’s start by considering a wheel with only 3 ducks. 
Before our first spin, we are in state 0, meaning that we have not collected any ducks. 
At this point, we spin the wheel and are guaranteed to collect a new duck, thus entering state 1. 
When we spin the wheel next, we have have a $\frac{1}{3}$ chance of remaining in state 1 and a $\frac{2}{3}$ chance of spinning a new duck that we have not collected before, thus entering state 2. Note that there no chance of returning to state 0, as no one will steal our beloved duck from us. Finally, once we enter state 2, we have a $\frac{2}{3}$ chance of remaining in state 2 and a $\frac{1}{3}$ chance of moving to state 3, in which wee have collected all of the ducks and we are finished!

This Markov process can be depicted graphically, where the nodes of the graph represent the state and the directed edges (arrows) of the graph represent the probability of transitioning from one state to another. 

INSERT IMAGE HERE.

Now, to estimate the expected amount of spins needed to collect all of the ducks, we need to estimate the expected number of steps required to move from state 0 in the markov chain to state 3. 
We will denote the expected number of steps to move from state $i$ to state $j$ in the Markov chain $E(i→j)$. 
We note that $E(i→i) = 0$ since we don’t need any spins to get to the state we are already in.

As mentioned above, after the first spin, we are guaranteed to be in state 1, from which then we need to get to state 3. 
We can write this as $E(0 → 3) = 1 + E(1 → 3)4$, leaving us to solve for $E(1 → 3)$. 

As discussed above, there is a $\frac{1}{3}$ chance of spinning and remaining in state 1 and a $\frac{2}{3}$ chance of spinning and getting to state 2. 
If we spin and remain in state 1, then our expected number of spins to get to the end will remain $E(1→3)$. 
However, if we spin and get to state 2, then our new expected number of spins until collecting all the ducks is $E(2→3)$. 
Thus, we can write: $E(1→3) = \frac{1}{3} * (1 + E(1→3)) + \frac{2}{3} * (1 + E(2→3))$. 
We can rearrange this equation to get $E(1→3) = 3/2 + E(2→3)$. 
This leaves us to solve $E(2→3)$, from which we can use the same logic to get: $E(2→3) = ⅔ * (1+E(2→3)) + ⅓ * (1+E(3→3))$, which we can once again rearrange to get $E(2→3) = 3 + E(3→3)$.

Now, to make things more clear, let us rewrite these equations in reverse order:
$$
\begin{align*}
E(3→3) &= 0 \\
E(2→3) &= 3 + E(3→3) \\
E(1→3) &= 3/2 + E(2→3) \\
E(0→3) &= 1 + E(1→3)
\end{align*}
$$

To solve for $E(0→3)$, we can plug the result from the first line into the second line, then the result of the second line into the third line, and repeat until we get a value for $E(0→3)$, which in this case is 5.5. 
Thus, we should expect to spin the wheel 5 or 6 times to collect the three ducks. 

OK, so this is great, but we are not done, since there are 12 ducks that we need to collect! 
We could repeat this process for a bigger Markov chain, but that would take a long time. 
Also, there might be 20 ducks next year, and we definitely don’t want to do this again in a year. 
So, let’s see if we can spot any patterns and get a general formula for any number of ducks!


