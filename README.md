# LinearRegressionModels

import numpy as np
from matplotlib import pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn import datasets
import pandas as pd

class LinearRegression:
    def __init__(self, le=0.001, iters=1000): #define class members 
        self.le=le
        self.iters=iters
        self.weights=None
        self.bias=None
        
    def fitting(self,X,y): #gradient descent
        n_samples, n_features=X.shape
        self.weights=np.zeros(n_features)
        self.bias=0
        
        for _ in range(self.iters):
            y_predicted=np.dot(X,self.weights)+self.bias
            
            dw=(1/n_samples)*np.dot(X.T,(y_predicted-y))
            dinter=(1/n_samples)*(np.sum(y_predicted-y))
            
            self.weights-=self.le*dw
            self.bias=self.le*dinter
            
    def predict(self,X): #get our Y_predicted based on LMS algorithm 
        y_predicted=np.dot(X,self.weights)+self.bias
        return y_predicted
        
X,y=datasets.make_regression(n_samples=100,n_features=5,noise=20,random_state=4)
X_train, X_test, y_train, y_test= train_test_split(X,y,test_size=0.2,random_state=1234)

regressor=LinearRegression(le=0.001)
regressor.fitting(X_train,y_train)
predicted=regressor.predict(X_test)

def MSE(y_true,y_predicted):
    return np.mean((y_true-y_predicted)**2) #cost function J

MSE_value=MSE(y_test,predicted)
print(MSE_value)

y_predict_line=regressor.predict(X)
y_predict_line.shape

y_dataframevalue=pd.DataFrame({"Y Actual":y_test,"Y Predicted":predicted,"Difference":y_test-predicted})

cmap=plt.get_cmap('magma')
fig=plt.figure(figsize=(8,6))
plt.scatter(y_test,predicted,color=cmap(0.9),s=10)
plt.title("Y predicted vs Y actual with N=100 & 5 Features")
