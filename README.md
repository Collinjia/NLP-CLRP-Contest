# CommonLit Readability Price

## NLP competition on Kaggle 
Rate the complexity of literary passages for grades 3-12 classroom use. Details instruction can be found [here](https://www.kaggle.com/c/commonlitreadabilityprize).

Input: [train.csv](train.csv). Since url_legal and license will be Null in the [test.csv](test.csv), so the only useful information is the excerpt that contains the text that needs to be rated. 
![image](https://user-images.githubusercontent.com/30295013/127888609-639481f1-7c5b-4823-9275-deb9ebbadcb7.png)

## EDA and Regression on features
First, I did some EDA and tried to use these EDA features to come up with a regression result. Detailed code in [CLRP_EDA](CLRP_EDA.ipynb).

This result is pretty bad **(RMSE = 0.944 on Final Score)** which only captures the numerical characteristics of the text.

## BERT, RoBERTa
Then, inspired by these notebooks:

1)    [Utilizing Transformer Representations Efficiently](https://www.kaggle.com/rhtsingh/utilizing-transformer-representations-efficiently)
2)    [Which representations are best?](https://www.kaggle.com/rajat95gupta/mean-pooling-4-seeds)
3)    [BERT - In Dept Understanding](https://www.kaggle.com/mdfahimreshm/bert-in-depth-understanding)

I used the RoBERTa model to be the word embedding method. Particularly, I used the Mean Pooling Model, which uses the average embedding across max length dimensions in the last hidden layer of RoBERTa. Detailed explanation in  [Utilizing Transformer Representations Efficiently](https://www.kaggle.com/rhtsingh/utilizing-transformer-representations-efficiently). 

In this model, I used the RoBERTa large model to be the pretrained model to fine tune.

Detailed Code in [CLRP_RoBERTa_Mean_Pooling_Fine_Tuing.ipynb](CLRP_RoBERTa_Mean_Pooling_Fine_Tuing.ipynb).  **(RMSE = 0.465 on Final Score)**

PS: 
1. You need to download the [RoBERTa large](https://huggingface.co/roberta-large) to do the fine tuning. In kaggle, you can find it [here](https://www.kaggle.com/maroberti/roberta-transformers-pytorch).
2. You might face the Runtime error shows CUDA out of memory. It means you don't have enough GPU RAM in your local  GPU, probably need around 13G. Try **Kaggle or Google Colab**.
3. It might take several hours to fine-tune this model, reducing the _number of seeds, epochs and folds_ can reduce the tuning time.
4. If you use Kaggle, you can use the model [clrp_mp](https://www.kaggle.com/ruoxijia/clrp-mp) that I trained. Just add it as the input dataset.

## RoBERTa + XGBRegressor
Then, inspired by this notebook [clrp-roberta-svm](https://www.kaggle.com/maunish/clrp-roberta-svm), I tried only to use the RoBERTa model as the embedding method and adaan a outside regression model to make the prediction. After trying out several regression methods, I chose the XGBRegressor.  **(RMSE = 0.477 on Final Score)**
    
PS: The embedding model is using the [model trained by others](https://www.kaggle.com/maunish/clr-roberta) since I didn't have enough GPU time to train another model without the regression head. However, you can get the embedding model by just removing the regression head in the previous mean pooling model. It might reach an even better result since the mean pooling model performs better than the pure attention model in this dataset.

Then I ensembled these two model and take the average prediction as the result. Detailed Code in [clrp-mp-xgb.ipynb](clrp-mp-xgb.ipynb).  **(RMSE = 0.462 on Final Score, Top 18%)**

## Top Rating Model
After the contest, some winners have public their solutions. Following are the links:

1. [1st place solution](https://www.kaggle.com/c/commonlitreadabilityprize/discussion/257844): Good use of external data
2. [2nd place solution](https://www.kaggle.com/c/commonlitreadabilityprize/discussion/258328): Interesting weight selection in ensembling.
3. [4th place solution](https://www.kaggle.com/c/commonlitreadabilityprize/discussion/258148): Averaging different seed during CV


