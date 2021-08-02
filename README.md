# CommonLit Readability Price

**NLP competition on Kaggle**: Rate the complexity of literary passages for grades 3-12 classroom use.

First, I did some EDA and tried to use these EDA features to come up with a regression result. Detailed code in [CLRP_no_NN](CLRP_no_NN.ipynb).

This result is pretty bad (RMSE = 0.884 on Public Score) which only capture the numerical characteristics of the text.

Then, inspired by these notebooks:

1)    [Utilizing Transformer Representations Efficiently](https://www.kaggle.com/rhtsingh/utilizing-transformer-representations-efficiently)
2)    [Which representations are best?](https://www.kaggle.com/rajat95gupta/mean-pooling-4-seeds)
3)    [BERT - In Dept Understanding](https://www.kaggle.com/mdfahimreshm/bert-in-depth-understanding)

I used the mean pooling RoBerta



https://www.kaggle.com/maunish/clrp-roberta-svm
