
# Lung Image Analysis: Tuberculosis Identification

This project uses lung image data to build a model that determines whether or not you have tuberculosis based on an x-ray of your lung. The goal would be to flag images as high or low priority to be reviewed by a physician. An algorithm of this type could lead to many benefits for those recieving healthcare and for the healthcare system as a whole. X-ray images are generally evaluated by a radiologist who, as a physician, has very valuable time. Reducing the amount of images a radiologist has to examine could lead to a cost reduction for patients and hospitals. Additionally, given how contagious and dangerous tuberculosis can be, being able to identify the presence of disease quickly may help prevent further infection or death. This can also be used as a proof of concept for using lung imagery to identify other diseases thay affect the lungs such as cystic fibrosis or COVID-19.

The dataset I used was obtained from the National Library of Medicine in collaboration with Shenzhen No.3 People's Hospital and contains 632 lung images. Half of the images display tuberculosis infections and and half do not.


```python
#Reading in data
from skimage.transform import resize
from skimage import io
import os 
import numpy as np
import matplotlib.pyplot as plt
#Specifying path to get data from 

path = r'C:/Users/Joe Krinke/Desktop/pulmonary-chest-xray-abnormalities/ChinaSet_AllFiles/ChinaSet_AllFiles/CXR_png/'
os.chdir(path)

#Create array to hold data
lung_images = []
labels = []

#Read in images and resize them
for i in range(326):
    number = str(i+1)
    number = number.rjust(4, '0')
    name = str(r'CHNCXR_') + number + r'_0.png'  #Create filename, padding the number in the center to match the format.[CHNCXR_0001_0] Last 0 indicates no disease.  
    unprocessed = io.imread(name,as_gray = True)
    resized = resize(unprocessed, (1000,1000)) #Resize images to standard size
    lung_images.append(resized)
    labels.append(0) #Add label indicating they are negative for disease. 
    
#Positive images go from 327 - 662 inclusive
for i in range(327,662):
    number = str(i+1)
    number = number.rjust(4, '0')
    name = str(r'CHNCXR_') + number + r'_1.png'  #Create filename, padding the number in the center to match the format.[CHNCXR_0001_1] Last 1 indicates disease.  
    unprocessed = io.imread(name, as_gray = True)
    resized = resize(unprocessed, (1000,1000))
    lung_images.append(resized)
    labels.append(1) 

#Convert data
lung_images = np.stack(lung_images)
```


```python
#Printing two sample classes to show the data. 

fig, (ax1, ax2) = plt.subplots(1,2, figsize=(10,5)) 
healthy = io.imread('CHNCXR_0001_0.png', as_gray=True) #Read in grayscale images
ax1.imshow(healthy, cmap ='gray',vmin=0, vmax=255) #Printing healthy image
ax1.set_title('Healthy Lungs')
ax1.set_xlabel('X')
ax1.set_ylabel('Y')
unhealthy = io.imread('CHNCXR_0555_1.png', as_gray = True)
ax2.imshow(unhealthy,cmap ='gray', vmin=0, vmax=255) #Printing unhealthy image
ax2.set_title('Unhealthy Lungs')
ax2.set_xlabel('X')
ax2.set_ylabel('Y')
plt.suptitle('Sample Images of Both Classes in the Dataset')
plt.show()
```


![Image of Diseased and Healthy Lungs](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/LungSamplePictures.PNG)



My target variable is a binary class where 1 indicates the presence of pneumonia and 0 indicates healthy lungs. I have a set of 662 images of lungs (half healthy and half unhealthy) along with their corresponding labels. Each image is approximately 2900x2900, but the sizes can vary so they needed to be resized to a standard size. Additionally, I decided to convert the images from RGB to grayscale in order to reduce the dimensionality of the data. This transformation is especially appropriate since x-rays are black-and-white images to begin with. The data is clean and has been properly labeled by physicians. No normalization is necessary as the numeric values the pixels can take have the exact same range [0-256].


I used logistic regression as my baseline model of performance. Logistic regression is simple and could work well if the data is linearly separable. The other model I plant to use is a random forest model, which will predict better if the decision boundary is more complex and non-linear.I chose to use a training and test dataset due to the large size of the image data. Each image, even after size reduction and conversion to grayscale, is very complex. Fitting random forest models for each fold could quickly become time-consuming and computationally intensive. Consequently, I felt it was appropriate to evaluate whether my model overfit using the training and test data alone.


```python
from sklearn.model_selection import train_test_split
from sklearn import linear_model
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from sklearn import metrics
#Divide data into training and test sets. 
xray_train, xray_test, xray_labels_train, xray_labels_test = train_test_split(lung_images, labels, test_size=0.20, random_state=42)
```


```python

#Preprocessing images to be used by the classifier
train_rows, train_numx, train_numy = xray_train.shape
xray_train_fix = xray_train.reshape((train_rows,train_numx*train_numy))
test_rows, test_numx, test_numy = xray_test.shape
xray_test_fix = xray_test.reshape((test_rows, test_numx*test_numy))
#Create logistic regression model as baseline. 

baseline = LogisticRegression()
baseline.fit(xray_train_fix, xray_labels_train)
xray_baseline_predict = baseline.predict_proba(xray_test_fix)[:,1]

#Create random forest model. 
lung_forest = RandomForestClassifier()
lung_forest.fit(xray_train_fix, xray_labels_train)
xray_forest_predict = lung_forest.predict_proba(xray_test_fix)[:,1]

```

    C:\Users\Joe Krinke\Anaconda3\lib\site-packages\sklearn\linear_model\logistic.py:432: FutureWarning: Default solver will be changed to 'lbfgs' in 0.22. Specify a solver to silence this warning.
      FutureWarning)
    C:\Users\Joe Krinke\Anaconda3\lib\site-packages\sklearn\ensemble\forest.py:245: FutureWarning: The default value of n_estimators will change from 10 in version 0.20 to 100 in 0.22.
      "10 in version 0.20 to 100 in 0.22.", FutureWarning)
    


```python
#Creating random guess classifier to use as baseline for comparison
def random_probs(y_values):
    guesses = np.random.rand(len(y_values),1)
    return (guesses)
```


```python


#Generate true positive rates and false positive rates across cutoffs
Rfpr, Rtpr, Rthresholds = metrics.roc_curve(xray_labels_test,random_probs(xray_labels_test))
BLfpr, BLtpr, BLthresholds = metrics.roc_curve(xray_labels_test,  xray_baseline_predict)
RFfpr, RFtpr, RFthresholds = metrics.roc_curve(xray_labels_test,xray_forest_predict)

#Plot ROC curves for both models
plt.plot(RFfpr,RFtpr, label= 'Random Forest Model')
plt.plot(BLfpr, BLtpr, label = 'Baseline Logistic Model')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve For Random Forest and Logistic Baseline')
plt.plot(Rfpr, Rtpr, label='Random Chance')
plt.legend(loc=4)
plt.show()

#print(metrics.roc_auc_score(xray_labels_test,  xray_baseline_predict),metrics.roc_auc_score(xray_labels_test,  xray_forest_predict ))
```


![ROC Curve](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/ROC_Curve.PNG)



```python
from sklearn.metrics import confusion_matrix
import seaborn as sn
import pandas as pd

#Evaluate confusion matrix on the test data
confusion_matrix_forest = confusion_matrix(lung_forest.predict(xray_test_fix), xray_labels_test)
confusion_matrix_df = pd.DataFrame(confusion_matrix_forest, index = [i for i in ('Positive', 'Negative')],
                  columns = [i for i in ('Predicted Positive', 'Predicted Negative') ])
plt.figure(figsize = (10,7))
plt.title('Confusion Matrix for Random Forest Classifier')
plt.xlabel('Predicted Labels')
plt.ylabel('Actual Labels')
sn.heatmap(confusion_matrix_df, annot=True)

```




    <matplotlib.axes._subplots.AxesSubplot at 0x2101894ed30>




![Confusion Matrix](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/ConfusionMatrix.PNG)


The random forest model performed fairly well overall, with a test set accuracy of 78 % and AUC of .827. This performance isn't terrible given the classes were equally balanced. However, the baseline logistic regression model was able to achieve a higher AUC of .897. This is likely due to the fact that there is a lot of noise in the image data. Some factors that could have contributed to this noise were that the images were resized from different original sizes, images were taken on patients who "filled up" the x-ray machine differently, and images were taken on people of different genders. Potential improvements to the modeling could include more complex image processing and feature extraction. Images could be cropped to eliminate blank space, features could be generated from 'influential' areas (in the center of the lungs). or one could apply techniques like PCA or Canny Edge Detection to pre-process the data. 

Different models may have also been able to fit the data better. It is possible that the flexibility of the random forest ended up being its downfall- it may have overfit due to high-dimensionality. Bagging is another approach that could reduce the correlation between individual trees and address this problem. I could also use convolutional neural networks, which implictly perform a form of feature extraction, as a modeling tool. However, I deliberately avoided using more complex techniques to try to see what kind of performance I could achieve with lower computational expenditure. 



**Citations:**

Jaeger S, Karargyris A, Candemir S, Folio L, Siegelman J, Callaghan F, Xue Z, Palaniappan K, Singh RK, Antani S, Thoma G, Wang YX, Lu PX, McDonald CJ.  Automatic tuberculosis screening using chest radiographs. IEEE Trans Med Imaging. 2014 Feb;33(2):233-45. doi: 10.1109/TMI.2013.2284099. PMID: 24108713

Candemir S, Jaeger S, Palaniappan K, Musco JP, Singh RK, Xue Z, Karargyris A, Antani S, Thoma G, McDonald CJ. Lung segmentation in chest radiographs using anatomical atlases with nonrigid registration. IEEE Trans Med Imaging. 2014 Feb;33(2):577-90. doi: 10.1109/TMI.2013.2290491. PMID: 24239990
