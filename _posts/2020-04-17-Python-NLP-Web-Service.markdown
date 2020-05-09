---
layout: post
title:  "Python +  NLP Web Service Classification & Clustering"
date:   2020-04-17 21:00:00
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---
<!-- MarkdownTOC -->

- [Some terms before headin:](#some-terms-before-headin)
- [**0. Prologue**](#0-prologue)
- [1. Web service](#1-web-service)
- [2. Project details](#2-project-details)
- [Post-Mortem](#post-mortem)
- [Reference](#reference)

<!-- /MarkdownTOC -->



### Some terms before headin:

> **Web Service Classification:**
> 
> Classification is a supervised learning process that requires the training dataset (in web service case is the functional description) and labels (web service category) to help web services users to predict the label of the given web services.

>**Web Service Clustering:**
>
> Clustering is an unsupervised learning process works for discovering and understanding the web services, which usually works for data preprocessing step for the classification.

> **NLP** (from Wiki):
> 
> Natural language processing (NLP) is a subfield of linguistics, computer science, information engineering, and artificial intelligence concerned with the interactions between computers and human (natural) languages, in particular how to program computers to process and analyze large amounts of natural language data.

### **0. Prologue**

 **This project implements web service classification and clustering using the given crawled data through ProgrammableWeb APIs. This projects performs:**
 - The comparision between different modeling approaches including **tf-idf**, **topic-modeling** and **word embedding.**
 - The comparison between different classification methods including **decision tree**,
**naïve bayes** classifier and **nearest neighbor** classifier.
- Perform cross validation for each type of classification to evaluate the
performance.
- Using Silhouette index to evaluate the clustering.
- The comparison between different clustering methods including **kmeans** and density-based spatial clustering (**DBSCAN**).
<br><br>

Crawled data record example:
```
http://www.programmableweb.com/api/the-global-proteome-machine$#$ The Global
Proteome Machine$#$Proteome data for biomedical research$#$4.4$#$ The Global
Proteome Machine$#$ The Global Proteome Machine$#$$#$The Global Proteome
Machine is an attempt to create knowledge from proteomics data and reuse it to solve
biomedical research problems. The Global Proteome Machine Database was built to
use GPM data to help validate peptide MS/MS spectra and protein coverage patterns.
The Global Proteome Machine Database API provides RESTful access to commonly
required information based on data from the GPM Database. Responses are JSON
formatted. $#$1$#$$#$$#$http://wiki.thegpm.org/wiki/GPMDB_REST$#$$#$2012-
12-17T09:51:40Z$#$$#$$#$http://api.programmableweb.com/apis/the-globalproteomemachine/
comments$#$database###science$#$Science$#$REST$#$http://gpm
db.thegpm.org/$#$$#$$#$JSON$#$$#$$#$$#$$#$$#$$#$$#$$#$$#$$#$$#$No$#$$#
$http://www.thegpm.org/$#$$#$$#$$#$$#$$#$$#$$#$2012-12-17T09:51:40Z
```

Web Service description (extracted as the **feature**):
```
The Global Proteome
Machine is an attempt to create knowledge from proteomics data and reuse it to solve
biomedical research problems. The Global Proteome Machine Database was built to
use GPM data to help validate peptide MS/MS spectra and protein coverage patterns.
The Global Proteome Machine Database API provides RESTful access to commonly
required information based on data from the GPM Database. Responses are JSON
formatted. 
```
Web Service Category (extracted as the **target variable** or **label**):
```
Science
```

<br><br>

### 1. Web service
**1.1. Tools and Libraries**

    Python 3.8.1 32-bit

    **Visual Studio Code**

    Flask

    Numpy

    Pandas

    Sklearn

    Gensim

    Jieba

    Nltk

    Tqdm
    
**1.2. Installation**
    1. unzip
    2. Open VSCode folder on the current directory
    3. Run it (it automately in venv.)
    4. As the server launched, open an browser
    5. Goto: http://127.0.0.1:5000/clustering/
            or
            http://127.0.0.1:5000/classification/


### 2. Project details

**2.1. Web Service Implementation**

**Main Class**
```py
from flask import Flask, url_for, request, redirect, render_template

#
# Classification
#
@app.route('/classification_results', methods = ['POST'])
def classification_results():
    classifier = request.form['model']
    data_model= request.form['data']
    file = request.form['file']
    
    pg = program.Main(classifier, data_model, file)
    return 'Successful!'
    

# Combine template and method
@app.route('/classification/')
def classification(name=None):
    return render_template('classfication.html')

    ...
    ...
    ...
if __name__ == '__main__':
   app.run(debug = True)
```

**Classification HTML Template**
```html
<html>
   <body>
      <form action = "http://localhost:5000/clustering_results" method = "post">
         Clustering Methods:
         <select name="model">
            <option value="KMeans">K-means</option>
            <option value="DBSCAN">DBSCAN</option>
         </select>
         <br>
         Data Model Approaches:       
         <select name="data">
            <option value="tf-idf">tf-idf</option>
            <option value="tm">topic-modeling</option>
            <option value="we">word-embedding</option>
         </select>
         <br>
         File Selection
         <select name="file">
            <option value="api.txt">api.txt</option>
            <option value="test.txt">test.txt</option>
            <option value="mashup.txt">mashup.txt</option>
            <option value="members.txt">members.txt</option>
            <option value="widgetfinal.txt">widgetfinal.txt</option>
         </select>


         <p><input type = "submit" value = "submit" /></p>
      </form>
   </body>
```

**2.2. Features Selection**

While analyzing the dataset before conducting any process, the author discovered that there are many features from the dataset are in **unique** value, such as "id"(links), "author", "commentsUrl" and so forth, which makes the built model in later steps being super overfitted (that's not what we want). So it is necessary to keep only the useful feature and lower down the overfitting possibilities. There the author decides to use only one feature - **description** from the first api.txt file, since the decription feature does the best job for express the important latent information about the data record. Therefore, the dataset would be 1-D dataset.

```py
        cat, des = [], []
        with open(file_name) as txtData:    
            lines = txtData.readlines()
            for line in lines:
                line = line.strip().split('$#$') 
                # keep the description
                des.append(self.remove_punctuation(line[7]))
                # keep the tag
                cat.append(line[18])
        df = pd.DataFrame(data = {'cat': cat, 'dess': des})
        
        #drop na
        df.dropna(axis=0, how='any', inplace=True)
        df[df.isnull().values==True]
        df = df[pd.notnull(df['dess'])]
        df = df.reset_index(drop=True)
```

**2.3. Data Balancing**

![05](https://i.imgur.com/TqsrhzV.png)
Obviously, the dataset are not well balanced, for the data records that belongs to *Tool* tag, there are more than 800 of them in the set. This would cause extremely long time for out later model to process and cause our prediction biased. Therefore, the author decides to trim the data out by only keeping *6* tags remained. 
![06](https://i.imgur.com/6QZIHHM.png)



**2.3. Documents Process**

**Words Lemmatization & *Stopword* Elimination**
```py
def preprocess_complete(text):
    """
        words lemmatization: verb passed tense to current tense
        @text: a string
        return: a string
    """
    result = ''
    for token in gensim.utils.simple_preprocess(text):
        if token not in gensim.parsing.preprocessing.STOPWORDS and len(token) > 3:
            result= result + lemmatize_stemming(token) + ' '
    return result
```
**Words Stemming**
```py
def lemmatize_stemming(text):  
    """
        Words stemming: cats -> cat; meeting -> meet
    """
    stemmer = SnowballStemmer('english')
    return stemmer.stem(WordNetLemmatizer().lemmatize(text, pos='v'))
```
The above two steps ensure the *word* extracted from the original dataset are normalized transformed to standardized format which help increase the modeling accuracy. 

```
              cat                                               dess                                                des
2       Messaging  blue is a service that stores text messages in...  [blue, servic, store, text, messag, convers, s...
3          Social  Gah People helps people from the FreeNode IRC ...  [peopl, help, peopl, freenod, channel, discov,...
4        Internet  This SOAPbased API allows the user to place a ...  [soapbas, allow, user, place, widget, websit, ...
5        Internet  The tel domain uses the DNS to store contact i...  [domain, use, store, contact, inform, push, da...
6          Social  100 Facts About Me is a Russian website dedica...  [fact, russian, websit, dedic, help, peopl, be...
...           ...                                                ...                                                ...
11189      Social  The Zutual API allows you programmatic access ...  [zutual, allow, programmat, access, zutual, pl...
11190      Social  A RESTful API that takes two twitter usernames...  [rest, take, twitter, usernam, return, json, l...
11193      Social  Zwiggo is a social platform designed for group...  [zwiggo, social, platform, design, group, user...
11194   Messaging  Zync is a global messaging platform that provi...  [zync, global, messag, platform, provid, commu...
11195  Enterprise  Zyncro is an enterprise social networking serv...  [zyncro, enterpris, social, network, servic, p...
```

Examples of the output for the docs before and after the process:
The *dess* reprensets the original doc, whereas the *des* represents the processed doc.
See the data record in "id == 2":  "is", "a" which the high frequently written words are eliminated.
```py
services/service - > servic
Restful/RESTful/REST/rest/... - > rest
```
**2.4 Cross-Validation**

```py
X_train, X_test, y_train, y_test = train_test_split(df['des'], df['cat_id'], random_state = 0,stratify=df['cat_id'])

```
**2.5. Data Modeling Approaches**
**TF-IDF:**

```py
    count_vect = CountVectorizer()
    tfidf_transformer = TfidfTransformer()

    # transform dataset into vectors
    X_train_counts = count_vect.fit_transform(X_train)

    # transform vectors into weighted vectors 
    X_train_tfidf = tfidf_transformer.fit_transform(X_train_counts)

```

```py
Results:
[[0. 0. 0. ... 0. 0. 0.]
 [0. 0. 0. ... 0. 0. 0.]
 [0. 0. 0. ... 0. 0. 0.]
 ...
 [0. 0. 0. ... 0. 0. 0.]
 [0. 0. 0. ... 0. 0. 0.]
 [0. 0. 0. ... 0. 0. 0.]] 
```

**LDA：**
```py
    count_vect = CountVectorizer()
    X_train_counts = count_vect.fit_transform(X_train)

    # use the trained doc vectors transform the test dataset into the vectors
    X_test_counts =  count_vect.transform(X_test)

    # initialize a LDA model
    # # of topics: 6
    lda_model = LatentDirichletAllocation(n_components=len(cat_id_df.values),
                                    learning_method='online',
                                    max_iter=20,
                                    random_state=42)
    
    # Use the LDA instance to model the dataset into the vectors of topics
    Xtopics = lda_model.fit_transform(X_train_counts)

    # transform the test doc into the vectors of topics
    Xtopics_test = lda_model.transform(X_test_counts)
```
```py
Results:
[[0.00416773 0.00417269 0.00422381 0.00417282 0.00416738 0.97909557]
 [0.00420153 0.12530752 0.36135089 0.00418112 0.00418709 0.50077185]
 [0.00308746 0.00311789 0.49922885 0.00308779 0.00309947 0.48837853]
 ...
 [0.17848864 0.00416919 0.77270219 0.03626544 0.00417198 0.00420257]
 [0.00245217 0.00245509 0.80951414 0.00245443 0.00245213 0.18067205]
 [0.0053782  0.27290006 0.70556677 0.00537981 0.00537642 0.00539873]]
```

**Word Embedding**
```py
    train_tagged = train.apply(
            lambda r: TaggedDocument(words=r['des'], tags=[r['cat']]), axis=1)
    test_tagged = test.apply(
            lambda r: TaggedDocument(words=r['des'], tags=[r['cat']]), axis=1)

    cores = multiprocessing.cpu_count()
    model_dbow = Doc2Vec(vector_size = 5, window = 2, min_count = 1, workers=cores)
    model_dbow.build_vocab([x for x in tqdm(train_tagged.values)])

    # training the neural network model 30 times for the better vectorerization accuracy
    for epoch in range(30):
        model_dbow.train(utils.shuffle([x for x in tqdm(train_tagged.values)]), total_examples=len(train_tagged.values), epochs=1)
        model_dbow.alpha -= 0.002
        model_dbow.min_alpha = model_dbow.alpha

    y_train, X_train = vec_for_learning(model_dbow, train_tagged)
    y_test, X_test = vec_for_learning(model_dbow, test_tagged)

def vec_for_learning(model, tagged_docs):
    sents = tagged_docs.values
    # transform the tagged docs into the vectors paragraph vectors
    targets, regressors = zip(*[(doc.tags[0], model.infer_vector(doc.words, steps=20)) for doc in sents])
    return targets, regressors
```
```py
# Results for tagged document:
TaggedDocument(['metro', 'publish', 'cloud', 'manag', 'build', 'help', 'media', 'compani', 'succeed', 'onlin', 'grow', 'readership', 'increas', 'revenu', 'build', 'brand', 'metro', 'publish', 'expos', 'rest', 'authent', 'base', 'oauth', 'specif', 'respons', 'json', 'format'], ['Enterprise'])
TaggedDocument(['pastebincom', 'websit', 'user', 'store', 'text', 'certain', 'period', 'time', 'site', 'programm', 'store', 'piec', 'sourc', 'code', 'configur', 'inform', 'kind', 'text', 'store', 'offer', 'function', 'site', 'includ', 'creat', 'list', 'delet', 'past', 'get', 'user', 'inform', 'set', 'use', 'rest', 'call', 'respons', 'format', 'document', 'requir', 'registr'], ['Tools'])
TaggedDocument(['coderwal', 'communiti', 'base', 'social', 'network', 'creat', 'purpos', 'allow', 'user', 'creat', 'lightweight', 'profil', 'display', 'achiev', 'cod', 'develop', 'user', 'gain', 'public', 'display', 'badg', 'trex', 'achiev', 'ampquothav', 'origin', 'repo', 'domin', 'languageampquot', 'octopusi', 'achiev', 'ampquothav', 'repo', 'follow', 'member', 'github', 'teamampquot', 'user', 'join', 'creat', 'compani', 'team', 'compet', 'highest', 'score', 'coderwal', 'profil', 'simpli', 'expos', 'json', 'represent', 'profil', 'retriev', 'http', 'valid', 'usernam', 'includ', 'request', 'respons', 'consist', 'profil', 'inform', 'list', 'achiev', 'return', 'json', 'jsonp'], ['Social'])
TaggedDocument(['circleci', 'platform', 'develop', 'creat', 'test', 'integr', 'deploy', 'code', 'project', 'circleci', 'offer', 'featur', 'like', 'push', 'code', 'github', 'deploy', 'code', 'server', 'circleci', 'allow', 'develop', 'access', 'integr', 'function', 'circleci', 'applic', 'exampl', 'method', 'includ', 'retriev', 'user', 'inform', 'retriev', 'project', 'inform', 'retriev', 'build', 'project'], ['Tools'])
```

**2.6. Classification**
Due to the reason that for different data model, the vectors vairables name are different, so here it uses X_trained (description), y_trained (category as the class), X_test and y_test.
**Decision Tree**
```py
        clf = tree.DecisionTreeClassifier()
        clf = clf.fit(X_trained, y_train)
        y_pred = clf.predict(X_test)
        print('Testing accuracy %s' % accuracy_score(y_test, y_pred))
        print('Testing F1 score: {}'.format(f1_score(y_test, y_pred, average='weighted')))
```
**Naive Bayes**
```py
    clf = MultinomialNB()
    clf = clf.fit(X_trained, y_train)
    y_pred = clf.predict(X_trained)
    print('Testing accuracy %s' % accuracy_score(y_test, y_pred))
    print('Testing F1 score: {}'.format(f1_score(y_test, y_pred, average='weighted')))
```
**Nearest Neighbors**
```py
    knn = KNeighborsClassifier()
    knn = knn.fit(X_trained, y_train)
    y_pred = knn.predict(X_trained)
    print('Testing accuracy %s' % accuracy_score(y_test, y_pred))
    print('Testing F1 score: {}'.format(f1_score(y_test, y_pred, average='weighted')))
```

**Evalution**
```
TFIDF + Decision Tree:
Testing accuracy 0.591
Testing F1 score: 0.590

TFIDF + Bayes:
Testing accuracy 0.640
Testing F1 score: 0.640

TFIDF + Nearest Neighbors:
Testing accuracy 0.446
Testing F1 score: 0.416



LDA + Decision Tree:
Testing accuracy 0.274
Testing F1 score: 0.274

LDA + Bayes:
Testing accuracy 0.345
Testing F1 score: 0.215

LDA + Nearest Neighbors:
Testing accuracy 0.301
Testing F1 score: 0.303



Word Embedding + Decision Tree:
Testing accuracy 0.477
Testing F1 score: 0.479

Word Embedding + Bayes:
Testing accuracy 0.411
Testing F1 score: 0.401

Word Embedding + Nearest Neighbors:
Testing accuracy 0.509
Testing F1 score: 0.513
```
**Brief Summary**
TF-IDF model has the best performance in pre-training on data vectorization when it is used in Decision Tree classifier or Bayes classifier. LDA (topic-oriented) produces the lowest accuracy. For Nearest Neighbors classifier, the word embedding model outstands the other two modelings.


**2.7. Clustering**
**KMeans**
```py
    # sets K equals to 6
    clf = KMeans(n_clusters=len(cat_id_df.values))
    s = clf.fit(X_train)
    labels = s.labels_
    print(labels)
    # using Silhouette distance to evaluate the clustering
    print(silhouette_score(X_train, labels))
```

**DBSCAN**
```py
    clf = skc.DBSCAN(eps=0.3, min_samples=10)
    s = clf.fit(X_train)
    labels = s.labels_
    print(labels)
    print(silhouette_score(X_train, labels))
```
**Evalution**
```
TFIDF + KMeans:
labels: [0 1 5 ... 1 3 1]
silhouette_score: 0.0090155443672946

TFIDF + DBSCAN:
error:
# label is 1

LDA + KMeans:
[0 2 2 ... 1 4 3]
0.41445914800038464

LDA + DBSCAN:
[0 0 0 ... 0 0 0]
0.46508006022380116


Word Embedding + KMeans:
[4 2 2 ... 4 1 2]
0.20370875

Word Embedding + DBSCAN:
error:
# label is 1

```
**Brief Summary**
The value of silhouette score from sklearn is in range between [-1, 1]. The coefficients near +1 indicates the sample is far away from the neighboring clusters. From the above output, TF-IDF has most distinctive clustering. However, there are program errors on both LDA and Word Embedding when using DBSCAN. 

### Post-Mortem
- For data preprocessing, there could be using other techniques to improve the feature selection. Like PCA...
- Data unbalanced data can be fixed by drop the data records from the more part.
- Parameters for initializing the models could be reconsidered.


### Reference
Docs:
https://pandas.pydata.org/
https://scikit-learn.org/
https://github.com/fxsjy/jieba

Theories:

https://cosx.org/2010/10/lda-topic-model/
https://www.jianshu.com/p/b16911338d39
https://www.zhihu.com/question/29901661
https://blog.csdn.net/weixin_41712499/article/details/82315207
http://blog.echen.me/2011/08/22/introduction-to-latent-dirichlet-allocation/
https://zhuanlan.zhihu.com/p/22332099
https://www.jianshu.com/p/2a76b7d3126b

Implementation:
https://www.jiqizhixin.com/articles/2018-03-05-3
https://blog.csdn.net/weixin_42608414/article/details/87967257
https://blog.csdn.net/weixin_42608414/article/details/87559437
https://github.com/tongzm/ml-python/blob/master/Topic_Model_Lda.ipynb
https://github.com/tongzm/ml-python/blob/master/Multi_Class_text_classification.ipynb
https://blog.csdn.net/a602232180/article/details/78586524
https://blog.csdn.net/zhuzuwei/article/details/80857446
https://blog.csdn.net/FAICULTY/article/details/79430164
https://zhuanlan.zhihu.com/p/20757320

