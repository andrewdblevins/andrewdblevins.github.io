---
layout: post
title: Beware of False Positive Accumulation
---

My first gig as a data scientist, at IMVU, I was tasked with building a random forest model to prevent credit card chargebacks. 
The model was pretty simple. It would build features about the transaction and the user and use those to estimate the 
probability of a chargeback. We had a large, beautiful dataset of previous chargebacks, because unfortunatly at the
time we were getting a lot of them. The model didn't need to be very complex to do a decent job, that was not the
interesting part of the project. 

The interesting part of the project was evaulating the cost of a false positive vs a false negative. In this case a 
false positive, declaring someone is a fraudster when they are actually a good customer, cost us the value of the transaction.

Each false negative resulted in an actual charegeback, costing us a constant fee, plus very steep (catastrophic) penalty 
if the percent of chargebacks got over a certain threshold.

Creating a loss function that balanced these costs and the risk of penalty was a lot of fun. We shipped a model, 
and our fraud rates when down. It was great. 

But it got me thinking about a deeper long term problem. After we shipped the model it was easy to see all of our false negatives, and there would be another campaign to address those. After all fraud is an adversarial game, and the fraudsters were going to adapt to our models.

So we would need to train a new model. But now our newest chargeback dataset was not a representitive sample of real data.
We would get to see all the false negatives, but all the transactions from our false positives, were being blocked by the model.
We would build a new model to block the chargebacks we were seeing. But naturally every model will have some false negatives 
some false positives. The false positives would accumulate with each model we stacked onto this mess, our chargebacks would
decrease, but we would have no visibility into the damage we were causing the company with the ever more and more false positives

Indeed, even the basic set of rules that our fraud analysts had implimented, could be thought of as a model, with false positives
and false negatives. Even my very first model was blind to the true amount the company was leaving on the table with our system
of fraud models.

Now, the obvious solution to this problem, is to randomly sample a very small portion of transactions and allow them to bypass
the entire fraud detection system. This would let us build up an unskewed dataset to compute the true value of our models,
and to truly train an improved model for the future. But just try convincing the business person incharge of fraud protection,
that sometimes we need to let people defraud us. It's not a fun conversation. Especially with that Visa Penalty hanging over the
company like the Sword of Damocles.

