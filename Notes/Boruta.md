# Boruta

Feauture selection is important, especially when there are hundreds of thousands of features that could introduce noise, in the process of building predictive models for an outcome. Boruta is an algorithm that helps with deciding whether a feature has some predictive value about the outcome.


1. There is no competition between features. Each feature competes with randomized versions of itself whcih are called shadow features. The threshold for a decision on whether the feature is important or not comes from the original and shadow features.

The importance of each original feature is compared with a threshold. The threshold is defined as the highest feature importance computed among the shadow features. When the importance of a feature is higher than this threshold, this is called a “hit”. The idea is that a feature is useful only if it’s capable of doing better than the best randomized feature.

2. Iterations provide the number of hits for each feature whcih follows a binomial distribution. The features to keep tentatively would be around the peak of the bell curve (~50 % hits). The features to drop and keep would be at tails.