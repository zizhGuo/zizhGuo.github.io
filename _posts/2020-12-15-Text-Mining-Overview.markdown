---
layout: post
title:  "(1/8)Overview - Text Mining for Multiclass Classification based on Yelp User’s Reviews"
date:   2020-12-15 18:50:00
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

### Text Classification of Yelp’s Category Based on Text Review
---
### Study Overview
Machine Learning and Deep Learning has become two of the most popular Artificial Intelligence topics across the globe in recent times, especially for being proved perform well targeting Natural Language Processing (NLP) Tasks. One foundational application of task is text mining. For the purpose of acquiring the solid ML and DL techniques, we choose to conduct the text classification experiment using Yelp Dataset. Not only we can apply our text mining theories to the actual business scenario, but to understand the model’s principles in-depth to enhance our skills. In our experiments, we have both trained many **classic textual models using W2V(SG/CBOW) and GloVe** and feed the document representation into the ML and DL models like **LR, SVM, RF, CNN, and RNN**. We have also tried implementing handcraft features and using validation accuracy to compare the experimental results. 

### Motivation
This study mainly focuses on Categorization in text mining topic, also as known text classification, which is one of the popular NLP tasks that is employed by most commercial entities. As the NLP tasks are applied in multiple individuals’ daily scenarios (i.e. user portrait classification, recommendation system, intelligent voice virtual assistant, etc.) **This study is first set up in an Independent Study course, hence the goals not only seeking the solutions to the text classification task but also to practice the current text mining techniques combing with ML and NN techniques to resolve the problem**. The study process includes **collecting dataset**, **preprocessing dataset**, **researching**, and **practicing current ML and NN models**, **tunning the models**, **retrieving the results**, and **producing summaries**. Meanwhile, the motivation also covers **toolkit** manipulation, **coding**, and engineering **implementation**. Hence, it is both endued the expectation in researching and engineering.

### Repo Architecture

|Directory|File|About|
|---|---|---|
|**dataset**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/dataset)|Introduing the dataset used in the study|
||IS_new.csv|Lemmatized dataset|
||IS.csv|Stemmed dataset|
|**experiment**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/experiment)|Giving the part of exact experiment details (more in README in ML_models and NN_models)|
||[run_ML.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/experiment/run_ML.py), [run_RNN.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/experiment/run_RNN.py)||
|**images**||Containing all README images|
|**ML_models**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/ML_models)|Giving the Machine Learning models details including the theory, parameters tuning, implementation.|
||[tuning_ML_models.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/ML_models/tuning_ML_models.py)||
|**NN_models (important)**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/NN_models)|1. Containing the detail information of the Neural Network models including Kim's **CNN model for text classification**, **RNN encoder model using GRU/LSTM** memory unit implemented by TensorFlow Keras; 2. Illustrating models' architectures, parameter's tuning process and implementation summary|
||[create_CNN_model.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/NN_models/create_CNN_model.py), [create_RNN_model.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/NN_models/create_RNN_model.py),[tuning_CNN_model.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/NN_models/tuning_CNN_model.py)||
|**plot**||Containing the methods to draw figure|
|**preprocessing**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/preprocessing)|The view of the preprocessed dataset|
||[stem_lemmatize.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/preprocessing/stem_lemmatize.py)||
|**textual_models (important)**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/textual_models)|1. Containing the report on the training/testing/tuning processes for W2V(SG/CBOW)/GloVe models using Gensim library; 2. Including the summary of the embedding models comparison and implementation; 3. Three **self-trained models/Word Vectors** and One **pretrained Stanford GloVe 42b 300d word vectors**|
||cbow_model.bin|CBOW model trained and saved by Gensim Word2Vec library|
||sg_model.bin|SG model trained and saved by Gensim Word2Vec library|
||glove_wv.bin|GloVe word embeddings trained by Stanford GloVe program|
||Stanford_glove.bin|Pretrained Stanford 42B300d Word Embeddings|
||[init_w2v_glove.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/textual_models/init_w2v_glove.py), [tuning_glove.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/textual_models/tuning_glove.py),[tuning_w2v.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/textual_models/tuning_w2v.py),[_lda.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/textual_models/_lda.py),[_tf-idf.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/textual_models/_tf-idf.py)||
|**regular_features**|[README.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/regular_features)|The method to extract the handcraft features|
||[hours.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/regular_features/hours.py)||
|**tools**|[old_report.md](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/tree/main/tools)|A sumary on data reading and converting|
||[files_read.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/tools/files_read.py), [statistics.py](https://github.com/zizhGuo/Text_Classification_Based_on_Text_Reviews/blob/main/tools/statistics.py)||