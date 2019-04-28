# Project: Twitter Sentiment Analysis
Note: Check jupyter notebook not python script (it auto generated from Google Colaboratory)
Download data from: https://s3.amazonaws.com/vikrantinterviewdata/model_data.zip
`wget https://s3.amazonaws.com/vikrantinterviewdata/model_data.zip`

## Few Methods Tried For Sentitment Analysis
### 1. Naive Bayes
#### Pros
- simple to train
- fast
- model size is small
- can work on sparse data

#### Cons
- features are independant
- word level probability distribution
- does not work well on twitter data

### 2. Doc2Vec + Multiclass Classification
#### Pros
- easy to train
- Doc2Vec model can be trained fast
- no need to train Doc2Vec model with every new data
- training classification model is fast
- method is split into 2 separate tasks and can be improved imdependantly
- Huge amount of data not required

#### Cons
- need to train Doc2Vec if domain changes or lots of new vocabulary is changed
- does not take dependencies into account
- a single work cannot change the vector document vector so much
- not best suited for Sentiment Analysis on short texts
- classification model not too complex hence cannot figure out dependencies

### 3. Language Models (Specefically ULMFiT) - Final Implemtation
#### Pros
- State of the Art Language  Model with Transfer Learning
- Can use pre-trained LM and Fine-Tune to the domain data
- Need only to Train Last few layers per domain
- Classiffiers can be bolted on the model 
- needs very few samples to achieve accuracy comparable to other SoTA models

#### Cons
- slow to train
- slow to predict single input
- fine tuning is required of last few layers for the said task
- lots of hyper-tuning options to try to get SoTA results

## Conclusion
- Comparing the model performance using accuracy metric it looks like its performaing very well with very hundreds of training samples instead of millions of samples required to train DNN models for the task. Language Models like ULMFiT allows a flexible tranfer learning mechanish as a cost of higher computation at runtime since language model needs to run to proces sentenc vector representation of the input text.
- Although accuracy metrics is not the best but we can be confident that the model is performaing good since its only getting confuesd a lot by positive and neutral tweets which is understandable since its hard for model to get sarcasm, analogy and context of the twitter post just from this data and training procedure.
- Even though the model accuracy is 90% we can still improve the model by few iteration of fine tuning until the train loss is higher than validation loss which gives indication that overfitting is not happening. But also hyper-parameter tuning is also required to get few more points in the perfermance.
- To make this model production ready we need to first figure out if its getting used realtime as the query comes or near realtime where batching can happen. The model performances increases a lot for inference when its done in batches since matrix multiplication is more effective and can be distributed. Also GPU is the intended device for training ang inference for production. Inference latency is poor for single query  predictions even on GPU. We can add multiprocessing but python is not bes suited for it.
- this notebook needs to converted to python script which should be easy but methods needs to be added for training or loading already trained models.
- the model might be exportable to common format like onnx so it can be served using a hig perfermance model server.

# NOTE
At the end due to some issue in fastai library i was not able to produce the confusion matrix for the evaluation data using batch processing. I was able to produce the correct predictions using single query loop but forgot to save it and it takes hours to generate again without batching.
