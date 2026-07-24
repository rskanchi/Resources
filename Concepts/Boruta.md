# Boruta

Feature selection is important when building predictive models for an outcome, especially when there are hundreds of thousands of features that could introduce noise. Boruta is an algorithm that helps with deciding whether a feature has some predictive value about the outcome. [Boruta R package](https://github.com/rskanchi/Resources/blob/main/Library/Boruta_Feature_Selection_2010_JStatSoftware.pdf) implements a novel feature selection algorithm using a Random Forest classifier to iteratively identify relevant variables by comparing them to randomly generated attributes. The algorithm determines importance based on Z-scores and statistical testing, removing irrelevant features and confirming important ones. The Boruta package offers convenient functions for accessing and interpreting results.  

1. There is no competition between features. Each feature competes with randomized versions of itself which are called shadow features. The threshold for a decision on whether the feature is important or not comes from the original and shadow features.

The importance of each original feature is compared with a threshold. The threshold is defined as the highest feature importance computed among the shadow features. When the importance of a feature is higher than this threshold, this is called a “hit”. The idea is that a feature is useful only if it’s capable of doing better than the best randomized feature.

2. Iterations provide the number of hits for each feature and follows a binomial distribution. The features to keep "tentatively" would be around the peak of the bell curve (~50 % hits). The features to drop and keep "for sure" would be at tails.

If you have time, please listen to the conversation below about the [Boruta article](https://github.com/rskanchi/Resources/blob/main/Library/Boruta_Feature_Selection_2010_JStatSoftware.pdf) created using NotebookLM.  

[Download ~12 min audio file from this folder](https://github.com/rskanchi/Resources/blob/main/audio/Boruta.wav)



-----------

# Boruta

## What is Boruta?

Boruta is a feature-selection algorithm used to identify variables that contain useful information for predicting an outcome. It is particularly helpful when a dataset contains many features, some of which may be irrelevant or contribute mostly noise.

Unlike minimal-optimal feature-selection methods, which seek a small subset of variables sufficient for accurate prediction, Boruta aims to identify all variables that are relevant to the outcome. The [Boruta R package](https://github.com/rskanchi/Resources/blob/main/Library/Boruta_Feature_Selection_2010_JStatSoftware.pdf) implements the algorithm using random-forest variable importance.

## How does Boruta work?

### 1. Create shadow features

Boruta creates a randomized copy of every original feature by shuffling its values. These randomized variables are called **shadow features**.

Because their relationship with the outcome has been destroyed, shadow features provide a reference for the importance that could arise by chance.

### 2. Compare original and shadow features

A random forest model is trained using both the original and shadow features. The importance of each original feature is compared with the highest importance observed among the shadow features. When an original feature performs better than this threshold, it records a **hit**.

The basic idea is:

> A feature should be considered relevant only if it consistently performs better than the best randomized feature.

### 3. Repeat the comparison

The procedure is repeated across multiple iterations. Boruta uses the number of hits accumulated by each feature to classify it as:

- **Confirmed**: consistently more important than the shadow features
- **Rejected**: consistently less important than the shadow features
- **Tentative**: insufficient evidence to make a clear decision

## How should the results be interpreted?

A confirmed feature contains information associated with the outcome beyond what would be expected from a randomized variable. However, this does not mean that the feature is causal or that it will necessarily improve prediction in an independent dataset.

Correlated features may also be selected together because Boruta searches for all relevant features rather than only a minimal predictive subset.

## What I want to remember

- Boruta compares real features with randomized versions of itself (shadow features)
- A feature records a hit when its importance exceeds that of the best shadow feature
- Repeated comparisons classify features as confirmed, rejected, or tentative
- Boruta seeks **all relevant features**, not necessarily the smallest optimal feature set

## Additional resource

The following audio discussion of the [Boruta article](https://github.com/rskanchi/Resources/blob/main/Library/Boruta_Feature_Selection_2010_JStatSoftware.pdf) was created using NotebookLM:

[Download the approximately 12-minute audio file](https://github.com/rskanchi/Resources/blob/main/audio/Boruta.wav)
