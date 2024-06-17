**Explainable Artificial Intelligence for Dependent Features: Additive Effects of Collinearity**<br>
The repositry presents the code of a novel explainable AI method that consider the collineairty.<br>
Here we only provided an example for a linear regression model, however the code is easy to modify to consider a classification model or with caterogrical variables.<br>
------------------------------------------------------------------<br>
```
import pandas as pd
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt
Data = pd.read_csv('data.csv') # the data used here is the same one uploaded in the repositry
dataplot = sb.heatmap(Data.corr()) 
# displaying heatmap 
fig = dataplot.get_figure()
plt.rcParams['figure.figsize'] = [9, 7]
fig.savefig("Reg_Corr.png", dpi=300)
plt.xticks(fontsize=20)
plt.yticks(fontsize=20)
plt.show() 
```
![Reg_Corr](https://github.com/amaa11/Additive-Effects-of-Collinearity/assets/43352625/f69e375d-2f4f-4431-8d4a-f6e525f097e6)

```
# Note: here we did not divid the data into training and test. Howver, the user has the ability to divide the data and extract the coeff value from the test data
X = Data[['F_1', 'F_2', 'F_3', 'F_4', 'F_5', 'F_6', 'F_7', 'F_8', 'F_9', 'F_10', 'F_11', 'F_12', 'F_13', 'F_14', 'F_15', 'F_16']]
y = Data[['Target']]
```
```
total_effect = []
m=0
for j in X.columns.to_list():
    effect = 0
    for z in X.columns.to_list():
        for i in X.columns.to_list():
            if i != j and z!= i:
                model = LinearRegression().fit(X[[i]], X[[z]])
                beta = round(model.coef_[0][0], 4)
                #print('Beta: ', beta)
                effect =  effect + beta
        #else:
            #print('not calculated')
    F1 = LinearRegression().fit(X[[j]], y['Target'])
    par_2 = round(F1.coef_[0], 4)
    total_effect.append(round(effect*par_2, 4))
    print('The effect of ', X.columns[m], round(effect*par_2, 4))
    m = m+1
```
Then, we creat a dataframe and sort based on the effect size
```
col = X.columns.to_list()
df = pd.DataFrame({'Features': col, 'Effect size': total_effect})
df.sort_values(by=['Effect size'], inplace=True, ascending=True, key=abs)
df.reset_index(inplace=True, drop=True)
df
```
Finally, we vitualise the list of the important features
```
colors = ['cornflowerblue' if e >= 0 else 'coral' for e in df['Effect size']]
plt.rcParams['figure.figsize'] = [7, 10]
plt.barh(df['Features'],df['Effect size'],color=colors,edgecolor='black')
plt.xlabel('Effect size', fontsize=14)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.savefig("Reg_NewXAI.png", dpi=300)
plt.show()
```
![Reg_NewXAI](https://github.com/amaa11/Additive-Effects-of-Collinearity/assets/43352625/941b8d24-dd8b-4517-a527-f7babb3462b4)


The effect size represents the change one unit in the feature of interest and the rest of the features in the model toward model prediction
