---
layout: post
title:  "Data Mining: Agglomeration Implementation"
date:   2020-04-01 21:40:12
author: Zizhun Guo & Martin Qian
category: Projects
categories: jekyll update
visible: 1
---
Team Project Github link:
[https://github.com/Qianjx/CSCI720_HW7](https://github.com/Qianjx/CSCI720_HW7)

This project is an implementation of Clustering method Aggolomeration.

Core Codes:
```py

Author: Martin Qian
def Agglomeration_Clustering(data_nodes):
    '''
    Agglomeration algorithm
    Starting with all elements as a single cluster
    Ending with only one cluster
    Returning a tree recording all middle clusters
    '''
    min_dist = np.inf
    count = 0
    for node1 in data_nodes:
        for node2 in data_nodes:
            dist = distance.euclidean(node1.center , node2.center)
            count+=1
            if dist < min_dist and dist != 0:
                min_dist = dist
                node_pair = node1, node2
    
    return(Merge(data_nodes, node_pair))

```

```py
Author: Martin Qian
class Agglomeration_Node:
    '''
    Node definition for the agglomeration cluster
    left: left child of this node
    right: right child of this node
    values: list of IDs that are same cluster
    label: the class this node is
    size: how many values are in the cluster
    center: center point of the cluster
    Right node's size should be smaller than left
    '''
    def __init__(self, left = None, right = None, values = None):
        self.left = left
        self.right = right
        if(left != None and right != None):
            self.values = left.values+right.values
            self.center = (left.center*left.size + right.center*right.size)\
                /(left.size+right.size)
            self.label = min(left.label,right.label)
            self.size = left.size + right.size
        else:
            self.values = [values['ID']]
            self.center = values[1:]
            self.label = values['ID']
            self.size = 1
```

### Part 1 (Guo, Zizhun): Using Cross-Correlation for Feature Rejection and Selection
Codes:
```py
import pandas as pd

# rule out ID column
column_names = df.columns[1:column_names_size]  
df_new = df.iloc[:, 1:column_names_size]

# use panda package to get correlation matrix
corr = df_new.corr()
```
The cc matrix:
![ccmaxtrix](https://i.imgur.com/jcg1WMc.png)

<!-- For Questions **e** **f** **g**
Code:
```py
    min_dist = np.inf
    min_dist_2nd = np.inf

    idx_min = np.inf
    idx_min_2nd = np.inf
    
    for attr1 in range(0, 20):
        dist_total = 0
        for attr2 in range(0, 20):
            dist_total += distance.euclidean(corr.values[attr1] , corr.values[attr2])
        dist = dist_total/20
        if dist < min_dist and dist != 0:
            idx_min_2nd = idx_min
            min_dist_2nd = min_dist
            idx_min = attr1
            min_dist = dist
    print(corr)
    print(min_dist)
    print("first leaset correlated with all others:" + str(column_names[idx_min]))
    print(min_dist_2nd)
    print("second leaset correlated with all others:" + str(column_names[idx_min_2nd]))
``` -->


**Questions:**

a. Which two attributes are most strongly cross-correlated with each other? ( ¼ )
> Corn & Tortya

b. Which attribute is fish most strongly cross-correlated with? ( ¼ )
> Eggs

c. Which attribute is meat most strongly cross-correlated with? (¼)
> Sauce

d. Which attribute is beans most strongly cross-correlated with? (¼)
> Meat

e. Which one attribute is least correlated with all other attributes? (¼)
> Milk

f. Which second attribute is least correlated with all other attributes? (¼)
> Fruit

g. If you were to delete two attributes, which would you guess were irrelevant? (¼)
> Milk & Fruit

h. If buying fish is strongly cross-correlated with buying cereal, and buying cereal is strongly crosscorrelated with buying baby products, is buying fish strongly cross-correlated with buying baby products? Can you explain this? (¼)
> Yes. Buying fish is also strongly cross-correlated with buying baby products.
> Suppose: 
$cov(buying fish) = x = cos\alpha$ 
$cov(buying cereal) = y = cos\beta$
$cov(buying baby products) = z = cos\gamma$
Since the value of x, y and z are in range between -1 to 1 
$\alpha, \beta, \gamma\in[0, \pi]$
so $cos\alpha cos\beta - sin\alpha sin\beta <= cos\gamma <= cos\alpha cos\beta + sin\alpha sin\beta$
so $cos(\alpha + \beta) <= cos\gamma <= cos(\alpha - \beta)$
so $\vert \alpha - \beta \vert <= \gamma <= min(\alpha + \beta, 2\pi - \alpha -\beta)$
This proves if the other sides from $\alpha$ and $\beta$ are at the same side of the common side which they both have (x and y are both either positive or negative), $\gamma$ has the minimal value of $\vert \alpha - \beta \vert$. $cos \gamma$ would be closer to 1.0 or -1.0 than $\alpha$ or $\beta$.
If $\alpha$ and $\beta$ are on the wrong sides, $\gamma$ can still get smaller value, which makes the correlation coefficients keep strongly the same.


---
### Part 3 (Qian Martin): Agglomeration

Final agglomeration clustering result:
![HW7](https://i.imgur.com/nVBgvFc.png)

---


**Martin Qian:**
>I learned a lot of clustering algorithms, not only agglomeration, I also learned something about k-means, DBScan and PCA. Apart from this, I learned the importance of effeciency and code optimization as this time training the model is really time consuming. The time complecity is O(n^3) and as a matter of fact can be reduced to O(n^2) if we can backup all these distance rather than compute them again.
> As for data exploration itself, I have to say the data is exactly divided into 6 clusters, very clearly and explicit. When all beanches of one node is in big size, this indicating this two nodes are all complete and pure clusters.  

**Zizhun Guo:**
>What did I learn?
CC can be used to determine what attributes can be discarded or not. For example, some attributes like milk and fruit attributes, they are the two attributes that are least correlated with all other attributes. The reason may come from that all customers conducted consumption would highly possible purchase these two products, which makes them less considerably matter to the clustering. So the two can be pruned before conducting the clustering algorithm in order to reduce the dimensions of the data set so that to increase the efficiencies.