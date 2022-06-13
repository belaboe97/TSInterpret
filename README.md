# TSInterpret

TSExplain is a Python library for interpreting time series classification.
The ambition is to faciliate the usage of times series interpretability methods. 

## 💈 Installation
```shell
pip install TSInterpre
```
You can install the latest development version from GitHub as so:
```shell
pip install https://github.com/jhoelli/TSInterpret.git --upgrade
```

Or, through SSH:
```shell
pip install git@github.com:jhoelli/TSInterpret.git --upgrade
```


## 🍫 Quickstart
The following example creates a simple Supported Vector Classifer based on tslearn and interprets the Classfier by creating a counterfactual.
For further examples check out the <a href="">Documentation</a>.

```python
from TSInterpret.data import load_data
import sklearn
import numpy as np 
import pandas as pd
from tslearn.datasets import UCR_UEA_datasets
from tslearn.svm import TimeSeriesSVC
from tslearn.preprocessing import TimeSeriesScalerMinMax

dataset='BasicMotions'
train_x,train_y, test_x, test_y=UCR_UEA_datasets().load_dataset(dataset)
train_x = TimeSeriesScalerMinMax().fit_transform(train_x)
test_x = TimeSeriesScalerMinMax().fit_transform(test_x)

model = TimeSeriesSVC(kernel="gak", gamma="auto", probability=True)
model.fit(train_x, train_y)
print("Correct classification rate:", model.score(test_x, test_y))

item=test_x[10].reshape(1,test_x.shape[1],test_x.shape[2])
shape=item.shape
y_target= model.predict_proba(item)

from TSInterpret.InterpretabilityModels.counterfactual.Ates import AtesCF

exp_model= AtesCF(model,(train_x,train_y),backend='SK', mode='time')

exp = exp_model.explain(item,0, method= 'opt')

array, label=exp
org_label=y_target
cf_label=label[0]
exp=array

exp_model.plot(item,org_label,exp,cf_label,figsize=(15,15))

```

## 🏫 Affiliations
<p align="center">
    <img src="https://upload.wikimedia.org/wikipedia/de/thumb/4/44/Fzi_logo.svg/1200px-Fzi_logo.svg.png?raw=true" alt="FZI Logo" height="200"/>
</p>
