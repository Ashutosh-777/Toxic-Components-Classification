# Toxic Comment Classification

### Table of Content
- Dataset Overview
- Data Preprocessing and EDA
- Model Fitting 
- Results

![367521329-74f6d261-d48d-48c6-840b-6b6c44b59ee0](https://github.com/user-attachments/assets/ca01b6c6-f57d-4de7-ba7a-5db71903713b)


### Dataset Overview
The threat of abuse and harassment online prevent many people from expressing themselves and make them give up on seeking different opinions. In the meantime, platforms struggle to effectively facilitate conversations, leading many communities to limit or completely shut down user comments. Therefore, Kaggle started this competition with the Conversation AI team, a research initiative founded by Jigsaw and Google.

As a group of students with great interests in Natural Language Processing, as well as making online discussion more productive and respectful, we determined to work on this project and aim to build a model that is capable of detecting different types of toxicity like threats, obsenity, insults, and identity-based hate. 

The dataset we are using consists of comments from Wikipedia’s talk page edits. These comments have been labeled by human raters for toxic behavior. The types of toxicity are:
- toxic
- severe_toxic
- obscene
- threat
- insult
- identity_hate

There are 159,571 observations in the training dataset and 153,164 observations in the testing dataset. Since the data was originally used for a Kaggle competition, in the test_labels dataset there are observations with labels of value -1 indicating it was not used for scoring.

### Data Preprocessing and EDA

Since all of our data are text comments, we wrote our own `tokenize()` function, removing punctuations and special characters, stemming and/or lemmatizing the comments, and filtering out comments with length below 3. After benchmarking between different vectorizers (TFIDFVectorizer and CountVectorizer), we chose TFIDFVectorizer, which provides us with better performance.

![image](https://github.com/user-attachments/assets/ec9c7584-2004-4fbf-b581-593977e205eb)


The major concern of the data is that most of the comments are clean (i.e., non-toxic). There are only a few observations in the training data for Labels like `threat`. This indicates that we need to deal with imbalanced classes later on and indeed, we use different methods, such as resampling, choosing appropriate evaluation metrics, and choosing robust models to address this problem.



### Model Fitting

#### Evaluation Metrics Selection
During the modeling process, we choose multiple different evaluation metrics to evaluate the performance of models based on the nature of our data:

- Recall
- F Score
- Hamming Loss

#### Basic Model Comparison
Using Multinomial Naive Bayes as our baseline model, we first used k-fold cross validation and compared the performance of the followingi three models without any hyperparameter tuning: Multinomial Naive Bayes, Logistic Regression, and Linear SVC. Logistic Regression and Linear SVC perform better than Multinomial Naive Bayes.

After checking how these models perform on the test data, we notice that Muninomial Naive Bayes does not perform as well as the other two models while Linear SVC in general out performs the others based on F1 score. 

![image](https://github.com/user-attachments/assets/71ca9e05-972d-4aa5-a819-d54eb7f5d27e)


Overall, without any hyperparameter tuning, LinearSVC performs the best initially.

#### Pipeline with Manual Hyperparameter Tuning
After accounting for the imbalanced data, the F1 score of Logistic Regression model has jumped to an average of 0.9479 while Linear SVC has jumped to 0.9515.

![image](https://github.com/user-attachments/assets/c3c4eeed-5c10-4fe9-b15a-86a278105d45)


#### Grid Search

With the help of grid search, we were able to find the "optimal" hyperparameters for the models and have reached an average of the best score of 0.9566 for Logistic Regression and 0.9585 for Linear SVC.


#### Ensembling
To ensemble different models, we firstly tried a few models based on tree boosting, then used a voting classfier to ensemble one of the boosting model with the basic models in previous parts. We get a F1 score of 0.973566 and Hamming Loss of 0.024639 using Ensembling.

![image](https://github.com/user-attachments/assets/7cd2e43c-7698-4427-98de-5696a213d657) 
![image](https://github.com/user-attachments/assets/12aa9b84-3de9-4969-bfaf-0e7691ac5c4b)


### Results
![image](https://github.com/user-attachments/assets/c7c59f13-2bab-4baf-b3d6-3c5c9b54218c)
In terms of evaluation metric, Linear SVC performs the best. But we believe after tuning hyperparameters for ensembling, we will get better results. Besides, Linear SVC trains model the fastest. Refering to interpretability, Linear SVC is also easier for the users to understand and has a simpler internal processing.
Therefore, we choose Linear SVC as our optimal model.

### Top and Bottom Features
![image](https://github.com/user-attachments/assets/27ef7f1a-4030-4d9c-ad1e-d986224b0d96)



