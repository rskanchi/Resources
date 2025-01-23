# Boruta

Feature selection is important when building predictive models for an outcome, especially when there are hundreds of thousands of features that could introduce noise. Boruta is an algorithm that helps with deciding whether a feature has some predictive value about the outcome. [Boruta R package](https://github.com/rskanchi/Resources/blob/main/Library/Boruta_Feature_Selection_2010_JStatSoftware.pdf) implements a novel feature selection algorithm using a Random Forest classifier to iteratively identify relevant variables by comparing them to randomly generated attributes. The algorithm determines importance based on Z-scores and statistical testing, removing irrelevant features and confirming important ones. The Boruta package offers convenient functions for accessing and interpreting results.  

1. There is no competition between features. Each feature competes with randomized versions of itself which are called shadow features. The threshold for a decision on whether the feature is important or not comes from the original and shadow features.

The importance of each original feature is compared with a threshold. The threshold is defined as the highest feature importance computed among the shadow features. When the importance of a feature is higher than this threshold, this is called a “hit”. The idea is that a feature is useful only if it’s capable of doing better than the best randomized feature.

2. Iterations provide the number of hits for each feature and follows a binomial distribution. The features to keep "tentatively" would be around the peak of the bell curve (~50 % hits). The features to drop and keep "for sure" would be at tails.

If you have time, please listen to the conversation below about the [Boruta article](https://github.com/rskanchi/Resources/blob/main/Library/Boruta_Feature_Selection_2010_JStatSoftware.pdf) created using NotebookLM.  

<audio controls>
  <source src="https://github.com/rskanchi/Resources/blob/main/audio/Boruta.wav" type="audio/wav">
  Your browser does not support the audio element.
</audio>

