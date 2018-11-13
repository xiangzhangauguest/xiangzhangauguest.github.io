---
title: "One hot encoding and cross validation in kaggle."
layout: post
category: [人生经验]
tags: [AI, kaggle]
excerpt: "kaggle竞赛中用sklearn实现one hot encoding 和cross validation。"
---

# One hot encoding
竞赛中需要对特定的feature进行one hot encoding，其他的feature保持不变。假设以"_cat"结尾的feature是category的，以"_bin"结尾的是binary的，

则下面代码会实现对这两种feature的one hot encoding。

```py
import pandas as pd

def one_hot_encoding(raw_train_df, raw_test_df):
    col_id = "id"
    col_target = "target"
    col_X = raw_train_df.columns.tolist()
    col_X.remove(col_id)
    col_X.remove(col_target)
    col_cat_X = [a for a in col_X if a.endswith("cat") or a.endswith("bin")]
    
    train_id = raw_train_df[col_id]
    train_target = raw_train_df[col_target]
    test_id = raw_test_df[col_id]
    
    combine_df = pd.concat([raw_train_df[col_X], raw_test_df[col_X]])
    combine_dummied_df = pd.get_dummies(combine_df, columns=col_cat_X)
    
    train_df = combine_dummied_df[:raw_train_df.shape[0]]
    test_df = combine_dummied_df[raw_train_df.shape[0]:]

    train_df = pd.concat([train_id, train_target, train_df], axis=1)
    test_df = pd.concat([test_id, test_df], axis=1)
    
    return train_df, test_df
```

# Cross validation

虽然sklearn有自己的cross_validation_score，但是有时还是不能满足要求（可能也是我没发现），比如用nomalized gini做score function的时候，比较的

是instance之间predict_proba的相对大小，因此需要自己实现一个cv和score function。

```py
from sklearn.model_selection import train_test_split, KFold

def gini(y, pred):
    g = np.asarray(np.c_[y, pred, np.arange(len(y)) ], dtype=np.float)
    g = g[np.lexsort((g[:,2], -1*g[:,1]))]
    gs = g[:,0].cumsum().sum() / g[:,0].sum()
    gs -= (len(y) + 1) / 2.
    return gs / len(y)


def gini_normalized(y, pred):
    return gini(y, pred) / gini(y, y)
    
cv_scores = []
k_fold = 5
kf = KFold(n_splits=k_fold, shuffle=True, random_state=100)
for train_index, test_index in kf.split(X_train):
    rf.fit(X_train.loc[train_index], y_train.loc[train_index])
    pred_val_cv = [a[1] for a in rf.predict_proba(X_train.loc[test_index])]
    cv_scores.append(gini_normalized(y_train.loc[test_index], pred_val_cv))
    print "One round done."
    
print "cv gini score: {}".format(np.mean(cv_scores))
```




