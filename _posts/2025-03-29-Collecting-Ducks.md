---
layout: post
title: "Collecting Ducks"
date: 2025-03-29
categories: math probability
usemathjax: true
---

One of the iconic parts of APS is the Physical Review journal ducks. 
During the career fair, APS has a spinning wheel that gives you one of 12 unique ducks, themed after different Physical Review journals.

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
We will denote the expected number of steps to move from state $i$ to state $j$ in the Markov chain $E(i \rightarrow j)$. 
We note that $E(i \rightarrow i) = 0$ since we don’t need any spins to get to the state we are already in.

As mentioned above, after the first spin, we are guaranteed to be in state 1, from which then we need to get to state 3. 
We can write this as $E(0 \rightarrow 3) = 1 + E(1 \rightarrow 3)$, leaving us to solve for $E(1 \rightarrow 3)$. 

As discussed above, there is a $\frac{1}{3}$ chance of spinning and remaining in state 1 and a $\frac{2}{3}$ chance of spinning and getting to state 2. 
If we spin and remain in state 1, then our expected number of spins to get to the end will remain $E(1 \rightarrow 3)$. 
However, if we spin and get to state 2, then our new expected number of spins until collecting all the ducks is $E(2 \rightarrow 3)$. 
Thus, we can write: $E(1 \rightarrow 3) = \frac{1}{3} * (1 + E(1 \rightarrow 3)) + \frac{2}{3} * (1 + E(2 \rightarrow 3))$. 
We can rearrange this equation to get $E(1 \rightarrow 3) = 3/2 + E(2 \rightarrow 3)$. 
This leaves us to solve $E(2 \rightarrow 3)$, from which we can use the same logic to get: $E(2 \rightarrow 3) = \frac{2}{3} * (1+E(2 \rightarrow 3)) + \frac{1}{3} * (1+E(3 \rightarrow 3))$, which we can once again rearrange to get $E(2 \rightarrow 3) = 3 + E(3 \rightarrow 3)$.

Now, to make things more clear, let us rewrite these equations in reverse order:

$$
\begin{array}{rcl}
E(3 \rightarrow 3) &=& 0 \\
E(2 \rightarrow 3) &=& 3 + E(3 \rightarrow 3) \\
E(1 \rightarrow 3) &=& 3/2 + E(2 \rightarrow 3) \\
E(0 \rightarrow 3) &=& 1 + E(1 \rightarrow 3)
\end{array}
$$

To solve for $E(0 \rightarrow 3)$, we can plug the result from the first line into the second line, then the result of the second line into the third line, and repeat until we get a value for $E(0 \rightarrow 3)$, which in this case is 5.5. 
Thus, we should expect to spin the wheel 5 or 6 times to collect the three ducks. 

OK, so this is great, but we are not done, since there are 12 ducks that we need to collect! 
We could repeat this process for a bigger Markov chain, but that would take a long time. 
Also, there might be 20 ducks next year, and we definitely don’t want to do this again in a year. 
So, let’s see if we can spot any patterns and get a general formula for any number of ducks!

To generalize, let’s say that there are $N$ ducks. 
Once again, we are guaranteed to get a new duck on the first spin. 
Now, on the second spin, there is a $\frac{1}{N}$ chance of spinning the duck you already have, thus remaining in state 1, and a $\frac{N-1}{N}$ chance of spinning a new duck and moving to state 2. 
Similarly, once you get two ducks, when you spin next, there is a $\frac{2}{N}$ chance of getting one of the two ducks you already have and a $\frac{N-2}{N}$ chance of getting a new duck and thus moving to state 3. 
This pattern keeps repeating itself, until we have collected every duck except one. 
At this point, we are in state $N-1$, and, on the next spin, there will be a $\frac{N-1}{N}$ chance of spinning a duck we already have and a $\frac{1}{N}$ chance of spinning the final duck that we don’t have yet. Once again, this process can be graphically visualized as a Markov chain.

INSERT IMAGE HERE

Now, using the same logic that we used for the 3 duck case, we see that $E(0 \rightarrow N) = 1 + E(1 \rightarrow N)$, $E(1 \rightarrow N) = \frac{1}{N} * (E(1 \rightarrow N) + 1) + \frac{N-1}{N} * (E(2 \rightarrow N) + 1)$, $E(2 \rightarrow N) = \frac{2}{N} * (E(2 \rightarrow N) + 1) + \frac{N-2}{N} * (E(3 \rightarrow N) + 1)$, …, $E(N-1 \rightarrow N) = \frac{N-1}{N} * (E(N-1 \rightarrow N) + 1) + \frac{N}{N} * (E(N \rightarrow N) + 1)$. 
Once again, we can rearrange all of these equations and write them in the opposite order (you should try this on your own) to get:

$$
\begin{array}{rcl}
E(N \rightarrow N) &=& 0 \\
E(N-1 \rightarrow N) &=& \frac{N}{1} + E(N \rightarrow N) \\
E(N-2 \rightarrow N) &=& \frac{N}{2} + E(N-1 \rightarrow N) \\
&...& \\
E(2 \rightarrow N) &=& \frac{N}{N-2} + E(3 \rightarrow N) \\
E(1 \rightarrow N) &=& \frac{N}{N-1} + E(2 \rightarrow N) \\
E(0 \rightarrow N) &=& 1 + E(1 \rightarrow N)
\end{array}
$$

Now we are getting somewhere! We can now see that $E(x \rightarrow N) = \frac{N}{N-x} + E(x+1 \rightarrow N)$ when $x < N$ and $E(N \rightarrow N) = 0$. Our $E(x \rightarrow N)$ equation gives us a relationship between one term in our sequence and the previous terms. This relationship is known as a [recurrence relation](https://en.wikipedia.org/wiki/Recurrence_relation). 

Thus, writing out the full series, we get

$$
\begin{array}{rcl}
E(0 \rightarrow N) &=& \frac{N}{1} + \frac{N}{2} + \frac{N}{3} + ... + \frac{N}{N-1} + \frac{N}{N} \\
&=& N \bigg(1 + \frac{1}{2} + \frac{1}{3} + ... + \frac{1}{N-1} + \frac{1}{N}\bigg)
\end{array}
$$

Plugging in $N=12$ into this formula gives $E(0 \rightarrow N) \approx 37.24$.

In fact, the series $1 + \frac{1}{2} + \frac{1}{3} + ... + \frac{1}{N-1} + \frac{1}{N}$ is known as the (truncated) harmonic series (footnote that it is truncated). The sum of these first $N$ terms of the harmonic series is known as the Nth harmonic number. Thus, the expected number of spins to spin all $N$ unique ducks is $N$ times the $N$th harmonic number.

After determining that each spin takes about 20 seconds, and that the line is usually 15 people long, each of use could spin the wheel once every 5 minutes. 
Therefore, for each of use to collect every duck, it would take a little over 3 hours! 

SHOULD WE TALK ABOUT THE DATA WE COLLECTED????

