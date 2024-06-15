**Explainable Artificial Intelligence for Dependent Features: Additive Effects of Collinearity**<br>
The repositry presents the code of a novel explainable AI method that consider the collineairty.<br>
Here we only provided an example for a linear regression model, however the code is easy to modify to consider a classification model or with caterogrical variables.<br>
------------------------------------------------------------------<br>
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
