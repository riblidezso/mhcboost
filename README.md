# Comparing the performance of Neural networks and Gradient boosted trees  , and a stacked ensemble of both in MHC class I binding prediction


---
### Background

The prediction of MHC peptide binding is an important problem in immunology. Data driven methods give best performance for this task because of complexity of the MHC molecules and the binding process. The most successful machine learning models are Artificial Neural Networks in this area.  I have seen articles describing SVM models, but nobody has tried gradient boosted trees (as far as I know).


In the last years a lot of Data Science competitions were won with a framework called Gradient Boosted Trees, especially and implementtion called XGBoost. These models very frequently outperform neural networks in general machine learning tasks. 

These facts gave me the idea to compare the perfomance of  Neural networks  and Gradient boosted trees and in MHC class I peptide binding prediction.

Neural networks and Gradient boosted trees are very different models, therefore ensembling them can give another boost to performance.


----
### Data

#### Source
I have downloaded MHC class I peptide binding data which has been used in this benchmark article ( http://www.ncbi.nlm.nih.gov/pubmed/25017736)  from the IEDB website.


#### Encoding 

Input data is very simply coded as the folowing:
- zero padded sequences to the longest peptide length
    - state of the art models use more sophisticated prediction on different length peptides, but for demonstration purpose I didn't want to recreate those.
- amino acids are encoded in a one hot scheme, or Blosum encoding
- original peptide lengths are included

#### Target 

I predict IC50 binding data transformed as the following (from netmhc articles):

$y = 1 - \log_{10}(IC50)/ \log_{10}(50000)$


---

### Models

#### Neural network

I use a neural network architecture similar to the one used in NetMHC.
- 1 hidden layer
- 5 units in the hidden layer
- tanh activations for hidden layer, sigmoid for the final layer, these are only guesses I have found no description of activation in NetMHC arrticles.

I use the Keras library. http://keras.io


#### Gradient boosted trees
Some resources about the model:
- http://xgboost.readthedocs.org/en/latest/model.html
- http://homes.cs.washington.edu/~tqchen/pdf/BoostedTree.pdf
    
I used the XGBoost software library. http://xgboost.readthedocs.org/en/latest/


#### Ensembling

I simply used the mean of different predictions, as described in several NetMHC articles.

---

### Methods


I have done nested 5-fold cross validation evaluations. All models use early stopping on validation data. Validation data is a random10% subset of the 4 folds of traning data.

The folds used were defined in the benchmark article as the group similarity (cv_gs) folds.



--- 

### Conclusions


The Gradient Boosted Trees outperforms Neural Networks. The ensemble of these 2 models adds a further improvement.

Note that my NN ensemble perform significantly worse than NetMHC. It can be because of the treatment of different peptide lengths, or other differences between the my ensemble and NetMHC. I'm not sure about this.


---


### Future Work

#### Adding Gradient Boosted trees to the NetMHC ensemble

I think adding Gradient Boosted trees to the NetMHC ensemble could further imporove its peforance as demonstrated in the notebook.


#### Further ideas for improvement:

- Hypterparameter optimization for xgb
- Sophisticated handling of different length peptides, see literature.
- Different NN architectures for alleles with different number of training data available.
- Get the MHC pseudosequence data and repeat the comparison with netmhcpan.
- (More realistic random peptides.)




----
#### Notes

- To run this notebook you need to 
    - download the dataset,and have keras,theno,xgboost,sklearn,pandas,numpy installed
    - or use the docker image for a ready enviroment
	- install docker
	- go to the docker folder and follow instructions
    
    
    
- This notebook can be run as it is. Just click run all. 
    - It takes some time but on a stronger computer it should be less than 30 minutes.
    
- It takes several hours to run this notebook!


----


This notebook was created by Dezso Ribli
