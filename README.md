**Explainable Artificial Intelligence for Dependent Features: Additive Effects of Collinearity**<br>
The repositry presents the code of a novel explainable AI method that consider the collineairty.<br>
Here we only provided an example for a linear regression model, however the code is easy to modify to consider a classification model or with caterogrical variables.<br>
------------------------------------------------------------------<br>
```
import pandas as pd
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt
# Note: here we did not divid the data into training and test. Howver, the user has the ability to divide the data and extract the coeff value from the test data
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
Then we creat a dataframe and sore based on the effect size
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
