---
layout: post
title: Day 18022018... taking off!
subtitle: First nearly-palindrome post, few days to go live
bigimg: /img/garden.jpg
tags: [personal, news]
---
This is a personal anecdotical foreword: you'll be able to jump straight to project site to try it, run it, collaborating in one-two weeks from now, so... stay tuned!


### The garden problem

When I started to work on this project, it was not a *project* at all.

I only wanted to apply to my wife garden some basic **optimization tecniques**, and the simple and fascinating ideas behind **Genetic Algorithms** seemed to be worth a couple of hours programming.

Basically she had about forty plants and flowers, and she wanted them all happy and safe at the best available place out of fifty locations.

### The garden non-solution

After spending a short but intense programming session to build a simple program, and having fed it with terrace morphology and plants needs, I ran generations over generations of individual (solutions) till I understood I couldn't pull out a better solution.

One third of her plants simply would have been out of position and would have died. Simply, it turned out that the terrace was too much shady! So, they have been gently spread to our friends and relatives. But that's another story :)

Lesson #1: I could have understood that a terrace facing North couldn't host most of my wife plants, without programming anything. But that's it, a programmer acts often for the sake of curiosity.

### The optimization chimera and magic numbers

Anyway, this happened some years ago, plants were then moved to a more comfortable South facing terrace.

The point was, and is, we can face almost every optimization problem, we can obtain and explore nearly a big (data) solution space of infinite paths with different **operations research** techniques, but still we need to be lucky, and sometimes we are, and on the other side we always need to be happy with a sub-optimal solution (hopefully not stuck to a local optimum).

This means implementing good and appropriate modeling to our problem, and setup *magic numbers* to decide when and how algorithms stops, or how distribute and request computation power in an elastic (scalable) fashion to increase solution space exploration speed.

And this is something can be engineered to perform well, no matter what the problem is.

### The ElastXY project

Putting together all the pieces, last summer (August 2017) I had the chance to have a look into **Apache Spark and SMACK stack**, and I understood I could learn something new, while realizing a little framework that could leverage **distributed computing and genetic algorithm** (which I never forgot), adding some sugar features like metadata based configurations, simple access APIs, and some DevOps to simplify deployment.

I also discovered Scala, which I used for though algorithm proof-of-concepts, but not used for implementing framework as I'm not enough robust yet (I'll rewrite code as long as I will be).

Despite it was a laboratory, it seemed that a potentially interesting framework could see the light.

That said, my adventure in building a **Distributed Genetic Algorithm** project began, and now I can say I found some of those magic numbers and "elasticity" I needed (beside my homemade cluster, next steps of course will be testing on **Amazon EMR** and **Google Dataproc**).

Along the road, I learnt basis of Apache Spark, rudiments of Scala language for quickly develop core parts of the algorithm on a sandbox, introduced Cassandra and Kafka for adding distributed capabilities, bought a mighty esacore home server which I happily stress, learnt virtualization techniques to build a simple cluster and finally... learnt how to publish a project and write a blog on GitHub :D

### Going open-source

By the way, the road to glory is long, especially if this recipe is homemade and realized overnight after daily job and family duties in a few months, so I decided to **share with community** this humble and really perfectible piece of code, and let's see what it comes.

Let you know in 1-2 weeks about my framework, just let me refine code and documentation to simplify the access, and do some family&friends trials.

Ah, I'm also working to a side-project, a site showcasing the four applications I realized firstly with my framework, which will be revealed when it's time.

Talk soon!

> In next posts, I will also explain why the butterfly and why I chose this name..

#### Disclaimer

It seems that many problems can be modeled and solved with genetic algorithms and evolutionary computing, and, given that resources are available on cloud with a modern serverless approach, they are almost infinite (limited by your wallet, in the end).

On the other side, as an engineer, I'm always attracted by magic computations and heuristics, but sometimes I don't see the appropriate scope. For example, TSP problem like Sudoku solving can be better treated with other algorithms.

Anyway, I'm not a data scientist, or a subject matter expert of everything, so I hope I'm excused and can concentrate on how to make it happen in a configurable, scalable, secure, etc., way :)
