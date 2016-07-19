# Comparing the performance of Neural networks and Gradient boosted trees  , and a stacked ensemble of both in MHC class I binding prediction


---
### Background

The prediction of MHC peptide binding is an important method in immunology. Data driven methods give best performance for this task because of complexity of the MHC molecules and the binding process. The most successful machine learning models were Neural networks in this area.  I have seen articles describing SVM models, but nobody has tried gradient boosted trees (as far as I know).


In the last years the vast majority of Data science competitions were won with a framework called Gradient Boosted Trees. These models very frequently outperform neural networks in gereal machine learning tasks. 

These facts gave me the idea to compare the perfomance of  Neural networks  and Gradient boosted trees and in MHC class I peptide binding prediction.

Also Neural networks and Gradient boosted trees are very different models, therefore ensembling them can give a really big boost to performance. So I tried to ensemble these models.


----
### Data

#### Source
I have downloaded MHC class I peptide binding data which has been used in this benchmark article ( http://www.ncbi.nlm.nih.gov/pubmed/25017736)  from the IEDB website.


#### Encoding 

Input data is very simply coded as the folowing:
- zero padded sequences to the longest peptide length
- amino acids are encoded in a one hot scheme
- species are encoded in a one hot scheme
- HLA types are encoded in a one-hot scheme
- HLA loci are encoded in a one-hot scheme
- HLA serotypes are encoded in a one-hot scheme
- peptide lengths are included

#### Target 

I predicted binary binding labels based on the IC > 500nM threshold.


---

### Models

#### Neural network

I used a neural network architecture similar to the one used in NetMHCpan.
- 1 hidden layer
- 60 units in the hidden layer

I used the Keras software library. http://keras.io


#### Gradient boosted trees
Some resources about the model:
- http://xgboost.readthedocs.org/en/latest/model.html
- http://homes.cs.washington.edu/~tqchen/pdf/BoostedTree.pdf
    
I used the XGBoost software library. http://xgboost.readthedocs.org/en/latest/


#### Ensembling



I used a stacking approach for ensembling. I used original features, and probabilities predicted by base models as input for the meta estimator. The same folds are used in both step to prevent label leakage.

Original article about stacking.
- http://www.sciencedirect.com/science/article/pii/S0893608005800231




---

### Methods


I made 5-fold cross validation evaluations, with AUC for all the measurements. All models can use early stopping on validation data so 3:1:1 scheme was used in each fold: 3 subsets for training 1 for validation and 1 for only prediction.



--- 

### Conclusions


Gradient boosted trees significantly outperform neural networks in this task. The ensemble of neural networks and gradient boosted trees gives further improvement over the base models. NetMHCPan performance is very similar to the ensemble model on this dataset.


Method | 5 fold CV
--- | --- 
NN | 0.9172
Gradient boosted trees | 0.9355
Ensemble of Gradient boosted trees + NN | 0.9377
NetMHCpan | 0.9376


The ensemble has better results than NetMHCpan on peptide length 8,10,11,12, but please note that these NetMHCpan results from the article were obtained with the L-mer approximation, not the newest representation on peptids of different length.

---


### Future Work

#### Training on similar data than NetMHCpan

The ensemble model performs comparably to NetMHCpan which is considered to be the state of the art model in MHC class I prediction. 

But NetMHCpan uses sophsticated encoding of the MHC molecules with pseudo sequences, a sophisticated way for predicting different length peptides and it is an ensemble of many models trained on different representations. 

**I think that Gradient Boosted trees trained on the same data used for NetMHCpan training, and using ensembling trees and neural networks could significanlty improve the current state of the art.**



---