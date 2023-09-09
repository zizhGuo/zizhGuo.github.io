---
layout: post
title:  "层级聚类探查游戏玩家类型 Agglomerative Clustering on Detecting Types of Gamers"
date:   2022-01-12 12:33:30
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

<br>

---

<br>
#### 1. Background and Goals

A large-scale study to examine the relationship between specific gaming experiences and teens’ civic activities and commitments, has been conducted by Pew Research Center in 2007. This dataset contains varied categories of features produced from thousands of teen respondents in the US, which help to reveal the level of fondness of teenagers across different video game genres.

There are 12 game genres. They are atomic, distinguishable and in binary values, share a similar status quo in the physical meaning, that by selecting and composing them, it would form different segments whereas each of them could represent a type of player group. A simple calculation of how many possible player groups: 2^12 = 4096. That said, most players are not favoring all genres, or at least most of them. Plus, we only have 1000+ respondents from the dataset.

Clustering for types of gamers based on game genres 

To discover the posible respondents segmentation to find the game genres pattern. The author decided to exploit Aggolomerative Clustering ML algorithm to get the data insights.  


#### 2. Description

|Feature|Game Genre|
|------|-----------------------|
|'k14a'|'fighting games'       |
|'k14b'|'puzzle games'         |
|'k14c'|'action games'         |
|'k14d'|'FPS games'            |
|'k14e'|'strategy games'       |
|'k14f'|'simulation games'     |
|'k14g'|'sports games'         |
|'k14h'|'RPG games'            |
|'k14i'|'adventure games'      |
|'k14j'|'racing games'         |
|'k14k'|'rhythm games'         |
|'k14l'|'survival horror games'|

#### 3. Statistics

<!-- 'k14a','fighting games'
'k14b','puzzle games'
'k14c','action games'
'k14d','FPS games'
'k14e','strategy games'
'k14f','simulation games'
'k14g','sports games'
'k14h','RPG games'
'k14i','adventure games'
'k14j','racing games'
'k14k','rhythm games'
'k14l','survival horror games' -->

###### Data Preprocessing

- Check out if the pandas DataFrame contains an anomaly or nan values. By looking out the unique values, we find that there are several anomalies (the answer is 9) and a few 8, but they are not many. Therefore, quickly drop these samples and in total the DataFrame has 1051 left (50 out).
- The original answer setting for k14 question is 1 for Yes 2 for No. Reset No to 0 for normalization, hence, to limit the mean score between the range of 0 to 1.

###### Statistic Description

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/statistics.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/statistics.png" alt="drawing" style="width: 70%;"/>
   </a>
   <figcaption>Table 1: Game Genres Statistics </figcaption>
</div>

From Table 1, k14b (puzzle games) and k14j (racing game) have the highest mean scores for voting, which indicates most respondents prefer them. It can be also noticed k14l (horror games) has the lowest mean scores which imply it is not popular at all.

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/hist_game_genres.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/hist_game_genres.png" alt="drawing" style="width: 70%;"/>
   </a>
   <figcaption>Fig 1: The distribution of Game Genres </figcaption>
</div>

Fig 1 shows a binary distribution trend that some groups of game genres are popular like puzzle games, racing games, sports games, action games and adventure games, while at the same time, the other groups of games are less like to be favored of like survival horror games, RPG games, FPS games and fighting games. 


<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/corr.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/corr.png" alt="drawing" style="width: 70%;"/>
   </a>
   <figcaption>Fig 2: Correlation Matrix for game genres </figcaption>
</div>

To further testify the grouping assumption, from Fig 8 correlation matrix, k14d (FPS games) and k14a (fighting games) are slightly correlated. Other pairs like k14c (action games) and k14d (FPS games) are just similar to k14d and k14a. The correlation matrix shows an interesting discovery, FPS games, fighting games, and action games may share some common traits that they might come from a same cluster and belongs to some type of player characteristics.


#### 4. Clustering

- To explore the relationship between different clusters and discover more possibilities of the clustering hierarchy, the Agglomerative Clustering method is used. The tool to implement clustering is Scikit-learn and SciPy.
- There are several options of clustering results based on the variable of distance threshold to choose. One possible clustering partitioning show below:

```py
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, fcluster

def plot_dendrogram(model, **kwargs):
    counts = np.zeros(model.children_.shape[0])
    n_samples = len(model.labels_)
    for i, merge in enumerate(model.children_):
        current_count = 0
        for child_idx in merge:
            if child_idx < n_samples:
                current_count += 1  # leaf node
            else:
                current_count += counts[child_idx - n_samples]
        counts[i] = current_count
    linkage_matrix = np.column_stack([model.children_, model.distances_,
                                      counts]).astype(float)
    dendrogram(linkage_matrix, **kwargs)


def clustering(df, linkage_matrix, n = 3):
    labels_ = fcluster(linkage_matrix, t=n, criterion='maxclust')
    n_labels = len(np.unique(labels_))
    clusters = []
    means = []
    index_columns = []
    label_counts = []
    for i in range(1, n_labels + 1):
        clusters.append(df[labels_ == i])
        means.append(np.mean(df[labels_ == i]))
        index_columns.append('cluster{}'.format(i))
        label_counts.append(len(df[labels_ == i]))

```

```py
    clusters = np.array(clusters)
    mean_matrix = np.around(means, 2)
    return clusters, index_columns, mean_matrix, label_counts


    model = AgglomerativeClustering(distance_threshold=0, n_clusters=None)
    model = model.fit(df)
    plt.title('Hierarchical Clustering Dendrogram')
    # plot the top three levels of the dendrogram
    linkage_matrix = plot_dendrogram(model, truncate_mode='level', p=7, color_threshold = 10)
    plt.xlabel("Sample Index")
    plt.ylabel("Distance(Ward)")

```
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/den7.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/den7.png" alt="drawing" style="width: 70%;"/>
   </a>
   <figcaption>Fig 3: Dendrogram for clustering </figcaption>
</div>

###### Clusters found and partitioning reason

There are 7 clusters in Fig 3 the dendrogram. To make sure the clustering has 7 clusters, the distance threshold has a relatively wide range to select. If the number of clusters moves to 5, 4 or 3, the distances threshold get larger ranged which would potentially merge the distinctive clusters and lose diverse persona images for different types of respondents gamers.

To further check the game genre preferences for the players in 7 clusters, the mean scores array for each cluster is produced in order to make a heatmap, this would help find the unique pattern for different types of players.

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/hm7.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/hm7.png" alt="drawing" style="width: 70%;"/>
   </a>
   <figcaption>Fig 4: Heatmap for mean game genre score given by respondents </figcaption>
</div>

###### Description for 7 clusters

1. Cluster 1: Hardcore gamers – specifically favor exciting games that help stimulate adrenaline. Horror and survival games are their favorites. They also play sports games, fighting games and action games. They do accept racing games and adventure games.
2. Cluster 2: Well-round gamers – play all sorts of games and are willing to try. However, their favorite game is strategy games. They are also in the explorer type player.
3. Cluster 3: Non-player – they do not play games quite often.
4. Cluster 4: Light gamers – they only play puzzles and rhythm games which apart themselves from other respondents. There are studies proving these gamers are more female targeting prone and mobile end incline.
5. Cluster 5:  Car, Gun & Ball gamers – as the counterpart cluster to Cluster 4, these gamers like player FPS games, action games, racing games and sports games. Studies existed they seem mostly male gender.
6. Cluster 6: Casual Gamers – these gamers don’t like fast-paced or thrill experiences but acceptable to all kinds of casual types of games including puzzle games, strategy games, sports games, adventure games.
7. Cluster 7: Role-play gamers – they like playing virtual character‘s life, enjoying having a second virtual life with characters they like in the games. They focus on experiences. They like playing adventure games and RPG games. 

#### 5. Summary

Based on reference: https://www.pewresearch.org/internet/2008/09/16/teens-video-games-and-civics/, the respondents top 3 games from © are listed in the table below:


<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/summary.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/summary.png" alt="drawing" style="width: 40%;"/>
   </a>
   <figcaption>Fig 5: 10 most frequently player games </figcaption>
</div>

From Fig 5, the top 3 games by number of votes are Guitar Hero (rhythm games), Halo3 (FPS games) and Madden NFL (sports games)

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/clusters.png">
   <img src="{{site.url}}/assets/2022-01-12-Teen-Game-Clustering/clusters.png" alt="drawing" style="width: 40%;"/>
   </a>
   <figcaption>Fig 6: Distribution of 7 clusters </figcaption>
</div>

- The top 4 types of gamers (top 4 clusters) based on the respondent's count are cluster2 (Well-round gamers), cluster5 (Car, Gun & Ball gamers), cluster6 (Casual Gamers) and cluster4 (Light gamers).
- For cluster2 respondents, they play all kinds of games and most sports games (0.95 mean score). For cluster5 respondents, they play sports games (0.82) as well and also FPS games (0.84). For respondents in cluster6 and cluster4, they play sports games and rhythm games a lot (0.74, 0.96).
- Therefore, the summation of the respondent counts of these clusters supports the top game report that the genres of the top 3 most mentioned games line up with the clustering results.




---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

