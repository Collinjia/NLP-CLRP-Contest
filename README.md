# CommonLit Readability Price

**NLP competition on Kaggle**: Rate the complexity of literary passages for grades 3-12 classroom use. Details instruction can be found [here](https://www.kaggle.com/c/commonlitreadabilityprize).

Input: [train.csv](train.csv). Since url_legal and license will be Null in the [test.csv](test.csv), so the only useful information is the excerpt which contains the text that need to be rate. 
![image](https://user-images.githubusercontent.com/30295013/127888609-639481f1-7c5b-4823-9275-deb9ebbadcb7.png)


First, I did some EDA and tried to use these EDA features to come up with a regression result. Detailed code in [CLRP_no_NN](CLRP_no_NN.ipynb).

This result is pretty bad (RMSE = 0.944 on Final Score) which only capture the numerical characteristics of the text.

Then, inspired by these notebooks:

1)    [Utilizing Transformer Representations Efficiently](https://www.kaggle.com/rhtsingh/utilizing-transformer-representations-efficiently)
2)    [Which representations are best?](https://www.kaggle.com/rajat95gupta/mean-pooling-4-seeds)
3)    [BERT - In Dept Understanding](https://www.kaggle.com/mdfahimreshm/bert-in-depth-understanding)

I used the RoBERTa model to be the word embedding method. Particularly, I used the Mean Pooling Model, which use the average embedding across max length dimensions in the last hidden layer of RoBERTa. Detailed explanation in  [Utilizing Transformer Representations Efficiently](https://www.kaggle.com/rhtsingh/utilizing-transformer-representations-efficiently). Detailed Code in [CLRP_RoBERTa_Mean_Pooling_Fine_Tuing.ipynb](CLRP_RoBERTa_Mean_Pooling_Fine_Tuing.ipynb).  (RMSE = 0.465 on Final Score)

PS: It might takes several hours to fine tune this model, reducing the number of seeds, epochs and folds can reduce the tuning time.

Then, inspired by this notebook [clrp-roberta-svm](https://www.kaggle.com/maunish/clrp-roberta-svm), I tried only use the RoBERTa model as the embedding method and add a outside regression model to make the prediction. After trying out several regression methods, I chose the XGBRegressor.  (RMSE = 0.477 on Final Score)

Then I ensembled these two model and take the average prediction as the result. Detailed Code in [clrp-mp-xgb.ipynb](clrp-mp-xgb.ipynb).  (RMSE = 0.462 on Final Score, Top 18%)



