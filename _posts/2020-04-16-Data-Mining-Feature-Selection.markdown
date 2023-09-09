---
layout: post
title:  "大数据分析课程：PCA和皮尔森相关系数 Data Mining: PCA + Pearson CC -> Feature Selection"
date:   2020-04-16 21:12:12
author: Zizhun Guo & Martin Qian
category: Projects
categories: jekyll update
visible: 1
---

<!-- MarkdownTOC -->

- [1. Feature Selection using Cross-Correlation](#1-feature-selection-using-cross-correlation)
        - [CC in three significant digits:](#cc-in-three-significant-digits)
        - [Projection Vector:](#projection-vector)
        - [Decision Boundary:](#decision-boundary)
        - [Feature Selection using CC:](#feature-selection-using-cc)
- [2. Brute Force Search for 2 Best Features](#2-brute-force-search-for-2-best-features)
    - [Question: What is the best classification accuracy this produces? What is the second best classification accuracy this produces?](#question-what-is-the-best-classification-accuracy-this-produces-what-is-the-second-best-classification-accuracy-this-produces)
- [3. Principal Components Analysis](#3-principal-components-analysis)
    - [PCA algorithm in sklearn](#pca-algorithm-in-sklearn)
        - [Eigenvectors of PCA\(let's take first 7 as example\):](#eigenvectors-of-pcalets-take-first-7-as-example)
        - [singular values of PCA](#singular-values-of-pca)
    - [What do the coefficients in the eigenvector tell you about which features are important?](#what-do-the-coefficients-in-the-eigenvector-tell-you-about-which-features-are-important)
- [4. Projection onto PCA](#4-projection-onto-pca)
- [5. Gradient Descent](#5-gradient-descent)
        - [Result of gradient descent](#result-of-gradient-descent)
- [6. Conclusions](#6-conclusions)

<!-- /MarkdownTOC -->
Team Project Link:
[https://github.com/Qianjx/CSCI720-HW9](https://github.com/Qianjx/CSCI720-HW9)

Data format used for this project:
![data](https://i.imgur.com/13S5MyY.png)

Goals:
1. Re-balance the Data
2. Feature Generation
3. Feature Selection using CC
4. Feature Selection Using PCA
5. Create a Linear 2D Classifier
6. Classify the Unknown data
7. Conclusion

#### 1. Feature Selection using Cross-Correlation

###### CC in three significant digits:
```py
Author: Zizhun Guo

def CC_analysis(data):
    '''
    Using correlation coefficient to analyze data
    '''
    CC_mat = data.loc[:, data.columns != 'Class'].corrwith(data['Class'])
    CC_mat = CC_mat.apply(lambda x: round(x, 3))
    return CC_mat
```

```py
# CC results
Age             -0.283
Ht              -0.010
TailLn          -0.266
HairLn          -0.095
BangLn           0.203
Reach           -0.092
EarLobes         0.043
TailLessHair    -0.222
TailLessBang    -0.340
ShagFactor      -0.515
TailAndHair     -0.259
TailAndBangs    -0.159
HairAndBangs     0.049
AllLengths      -0.157
ApeFactor       -0.537
HeightLessAge    0.193
dtype: float64
```
Feature1: **ApeFactor**; CC = **-0.537**
Feature2: **BangLn**; CC = **0.203**

###### Projection Vector:
So the projection vector should be the vector from the center of the attribute ApeFactor to the center of the BangLn
```py
Author: Martin Qian

    plt.plot([CA[0],CB[0]],[CA[1],CB[1]],\
        'm-' , label = 'projection vector')
    x = range(2 , 8)
```
###### Decision Boundary:
Dicision boundry is the vertical line of thr projection vector.
```py
    plt.plot(x, -(CA[0]-CB[0])/(CA[1]-CB[1]+0.)*(x-(CA[0]+CB[0])/2.)+(CA[1]+CB[1])/2. ,\
         'k-', label='decision boundry')
```

###### Feature Selection using CC:
```py
Author: Zizhun Guo

def feature_selection_using_CC(CC_mat, data):
    '''
    using CC to select two features that has the greatest abs and has opposite sign
    using LDA to test Accuracy
    '''
    print("cross correlation matrix:\n" + str(CC_mat))
    # assign the feature by abs sort and choose the greates
    feature1 = CC_mat.abs().sort_values(ascending=False).index[0]
    if CC_mat[feature1] < 0: 
        feature2 = CC_mat.sort_values(ascending=False).index[0]
    else: 
        feature2 = CC_mat.sort_values(ascending=True).index[0]
    print(str(feature1) +' '+ str(feature2) + ' Selected')
    plot(data, feature1, feature2)
    
    # LDA classify the trainning data
    clf = LinearDiscriminantAnalysis(solver = 'eigen', n_components = 1)
    clf.fit(data.loc[: ,[feature1, feature2]], data['Class'])
    curr_score = clf.score(data.loc[: ,[feature1, feature2]], data['Class'])
    print("Accuracy for these features:"+ str(round(curr_score, 3)))
```
![feature_selection](https://i.imgur.com/An6KPTT.png)
<center> Image1: Feature Selection Scatter Plot </center>

**Results (Accuracy for this classifier):** **0.753**

#### 2. Brute Force Search for 2 Best Features
```py
Author: Martin Qian

def BFS_analysis(data):
    '''
    Using Brute Force Search to analyze data
    '''
    score = 0
    score_second = 0
    for feature1 in data.columns:
        for feature2 in data.columns:
            if feature1 != feature2 and feature1 != 'Class' and feature2 != 'Class':
                # LDA classifier
                clf = LinearDiscriminantAnalysis(solver = 'eigen', n_components = 1)
                clf.fit(data.loc[: ,[feature1, feature2]], data['Class'])
                curr_score = clf.score(data.loc[: ,[feature1, feature2]], data['Class'])
                if( curr_score > score):
                    score_second = score
                    score = curr_score
                    features = [feature1 , feature2]
                    best_clf = clf
    return features, score, best_clf
```
So the highest score is achieved with the following features
Feature1: **ShagFactor**
Feature2: **ApeFactor**

![lda_scatter](https://i.imgur.com/P6YVlbz.png)
<center> Image2: Bruce For Search Best Classifier Scatter Plot </center>

**Results (Accuracy):** **0.817**

##### Question: What is the best classification accuracy this produces? What is the second best classification accuracy this produces?

**Best: 0.817**
**Second Best: 0.778**

#### 3. Principal Components Analysis

##### PCA algorithm in sklearn

```py
    pca = PCA(n_components = 7)
    pca.fit(data.loc[:, data.columns != 'Class'])
    print("eigen vectors of PCA\n"+str(pca.components_))
    print("singular values of PCA\n"+str(pca.singular_values_))
```
###### Eigenvectors of PCA(let's take first 7 as example):
```py
[[-0.023 -0.58  -0.046 -0.006 -0.004 -0.582 -0.003 -0.041 -0.042 -0.001
  -0.052 -0.051 -0.01  -0.056 -0.002 -0.557]
 [ 0.723  0.19   0.128  0.019 -0.005  0.227  0.003  0.109  0.133  0.023
   0.147  0.123  0.014  0.142  0.037 -0.533]
 [-0.249 -0.146  0.335  0.093  0.077 -0.133  0.002  0.241  0.258  0.017
   0.428  0.411  0.17   0.505  0.012  0.103]
 [ 0.066  0.041 -0.21   0.269  0.25   0.012 -0.02  -0.479 -0.46   0.019
   0.06   0.041  0.52   0.31  -0.03  -0.025]
 [-0.183 -0.309 -0.02   0.102 -0.093  0.441 -0.011 -0.123  0.073  0.196
   0.082 -0.114  0.009 -0.012  0.75  -0.125]
 [ 0.037  0.095 -0.001  0.288 -0.312 -0.15   0.006 -0.289  0.311  0.6
   0.287 -0.314 -0.024 -0.026 -0.245  0.058]
 [-0.003 -0.005 -0.006  0.005  0.006  0.003  0.    -0.01  -0.011 -0.001
  -0.001  0.     0.01   0.005  0.009 -0.003]]
```
###### singular values of PCA
```py
[931.941 573.253 457.488 260.495 109.357  62.967  10.034]
```
##### What do the coefficients in the eigenvector tell you about which features are important?

First of all, by the definition of the Principal Components, the eigen vector with the greater singular values works as a better vector to project and have a better effect on seprating the data.
Secondly, the eigen vector stands for the coefficient multipled by the features of raw data, so apparently, the more its absolute value is, the more importance the corresponding feature has. 
However, we know that PCA is used to do dimension reduction. At the same time, we only have 6 features at first and the rest are the linear combinations of these 6 features. As the results, these new feature should have no impact on the final eigenvectors. (Because the rank of the data is 6, it would break mathematic rules if it has more than 6 eigen vectors. So the result also supprted my opinion as eigen vectors that are more than 6 are nearly all 0s)

 
#### 4. Projection onto PCA
Since the first two singular values are the greatest among all, project the data onto the first two Principal Components.

![pca](https://i.imgur.com/mTcVZky.png)
<center> Image3: PCA Scatter Plot using first two eigen vectors</center>

On the other hand, I also get the projection scatter plot with only original dataset as following. And these two graphs are quite similar(if we ratote one of them by 180 degrees we can see distribution similarity).

![pca_1](https://i.imgur.com/xl8Z41K.png)
<center> Image4: PCA Scatter Plot using original dataset</center>


#### 5. Gradient Descent
```py
theta, rho =  Gradient_Descent__Fit_Through_a_Line_v100(\
        [data[features[0]],data[features[1]],data['Class']] , 62.7, 11.2 ,4.5)
```
By calling the method from HW8 and modifying their parameters( 62.7 is initial theta, 11.2 is initial rho and 4.5 is initial alpha, they can be calculated mannually with two centers of selected features), we could use gradient descent for this question. We set the loss function as the misclassification rate. The initial features we selected are ApeFactor and ShagFactor, we believe they are more reasonable since firstly their correlaton coefficients' absolute are the greatest, though both are negative; secondly they are selected as the features from the brute force algo part.

![gd](https://i.imgur.com/94010eq.png)
<center> Image5: Boundry given by GD</center>

###### Result of gradient descent
```
starting point:0.458 * x + 0.889 * y = 11.2(theta = 62.7)
final result: 0.781 * x + 0.6241 * y + -6.09 = 0   
Accuracy is 0.817
```
The results are interesting, since the accuracy the model produced is very closed to the one given by the Linear Discriminant Analysis. So we believe this is the limitation linear classifier that can achieve on this dataset.

#### 6. Conclusions
Done by all works above, the final classifier that was chosen by us is the decision boudary conducted by the **gradient descent** algorithm, since it provides the highest accuracy in the prediction task on the unclassified dataset.