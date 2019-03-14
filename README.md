# Stella 
Stella is **Nexus's Capital** *proprietary* a Trading Algorithm / Pipeline. 

This specific algorithm is composed of two parts: 
* DenseNet CNN 
* Trade executing algorithm 

## DenseNet CNN
A DenseNet Convolutional Neural Network is the specific Neural Network architecture we used for Stella. This specific model has been trained on **28x28** images of time-series currency exchange data. 

### 28x28 
We chose a **28x28** as it represents **28 features** for **28 time-window**. Each *row* represents a new day, while each *col* represents each feature for that given day (per stock)

#### 28 Features
The **28 Features** we have selected are as follows:

```python
# tuple: first value = parameters, second value = features
features = {
    tanh(stationary(close)): (0, 1),
    volume: (0, 1),
    RSI: ([15,20,25,30], 4),
    SMA: ([15,20,25,30], 4),
    MACD: ([[26,12],[28,14],[30,16]], 3),
    MACD_Trigger: ([[9,26,12],[10,28,14],[11,30,16]], 3),
    William_R: ([14,18,22], 3),
    Stochastic_Oscillator: ([14,18,22], 3),
    Ultimate_Oscillator: ([[7,14,28],[8,16,22],[9,18,36]], 3),
    MFI: ([14,18,22], 3)
}
```
### Normalization
We take the **Close** of the day and apply:
* Use **First Difference Method**
* Apply **tanh** 

This will create *stationary prices* between {-1,1}. This allows us to build a **Regression Model** on top of **Classification**

### Labeling
For **Labeling** we perform a **Sliding Window Approach** one day at a time. Therefor each **image** is **labeled** with the **following day of last day** so: 29th day. 

The reason we select 29th day is because we want to forecast the trend for the next day on any given day. 

### Regression
To perform **Regression** we will map prices between {-1,1} using the **tanh** function. 

### Classification
To perform **Classification** we will do the following:

```python
if price > 0:
    prediction = "buy"
else:
    prediction = "sell"
```

This is possible because of: 
* Normalization
* **tanh** 