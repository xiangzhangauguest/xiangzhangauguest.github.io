---
title: "Sklearn gridsearchcv and make_scorer"
layout: post
category: [人生经验]
tags: [AI, kaggle]
excerpt: "kaggle竞赛中用sklearn实现 gridsearchcv 和利用make_scorer传递customed score funtion."
---

# gridsearchcv
以xgboost为例，首先设定好不变的参数，然后设定变的参数的范围，最后利用gridsearchcv计算每组变量组的score, 用来选择参数，即model selection.

```py
from sklearn.model_selection import GridSearchCV

param_gsearch = {"max_depth": range(3, 10, 2), 
                 "min_child_weight": range(1, 6, 2)}

param = {"learning_rate": 0.2, "n_estimators": 20, "max_depth": 5, "min_child_weight": 1, "gamma": 0, "subsample": 0.8, "colsample_bytree": 0.8,
         "objective": "binary:logistic", "n_jobs": 2, "scale_pos_weight": 1, "random_state": 2017}
clf = XGBClassifier(kwargs=param)

# verbose > 0会打印cv过程的信息，便于观察.
gsearch = GridSearchCV(estimator=clf, param_grid=param_gsearch, scoring=gini_scorer, n_jobs=2, iid=False, cv=5, verbose=5)
gsearch.fit(train_df[col_X], train_df[col_target])
```

# make_scorer

注意到上面gridsearchcv的时候，有参数scoring=gini_scorer, 这里gini_scorer是custom score function，方便在cv的时候用自己的score function判定模型的好坏。 

具体用法如下：

```py
def gini(y, pred):
    g = np.asarray(np.c_[y, pred, np.arange(len(y)) ], dtype=np.float)
    g = g[np.lexsort((g[:,2], -1*g[:,1]))]
    gs = g[:,0].cumsum().sum() / g[:,0].sum()
    gs -= (len(y) + 1) / 2.
    return gs / len(y)


def gini_normalized(y, pred, pred_use_col):
    pred = [a[pred_use_col] for a in pred]
    return gini(y, pred) / gini(y, y)

from sklearn.metrics import make_scorer

gini_scorer = make_scorer(gini_normalized, greater_is_better=True, needs_proba=True, pred_use_col=1)
```

这里需要注意的是[make_scorer]接受的参数，最后是**kwargs，接受其他custom score function需要的参数，这个例子里是pred_use_col，用来取
predict_y的第二列作为predict_result，与ground truth共同计算score function，用法就是gridsearchcv中```scoring=gini_scorer```。

[make_scorer]: http://scikit-learn.org/stable/modules/generated/sklearn.metrics.make_scorer.html





