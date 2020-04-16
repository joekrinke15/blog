---
layout: post
title: Identifying Tuberculosis Using Lung Images
image: https://www.medicaldevice-network.com/wp-content/uploads/sites/11/2019/10/shutterstock_1117647020.jpg
---
# Lung Image Analysis: Tuberculosis Identification

This project uses lung image data to build a model that determines whether or not you have tuberculosis based on an x-ray of your lung. The goal would be to flag images as high or low priority to be reviewed by a physician. An algorithm of this type could lead to many benefits for those recieving healthcare and for the healthcare system as a whole. X-ray images are generally evaluated by a radiologist who, as a physician, has very valuable time. Reducing the amount of images a radiologist has to examine could lead to a cost reduction for patients and hospitals. Additionally, given how contagious and dangerous tuberculosis can be, being able to identify the presence of disease quickly may help prevent further infection or death. This can also be used as a proof of concept for using lung imagery to identify other diseases that affect the lungs such as cystic fibrosis or COVID-19.

The dataset I used was obtained from the National Library of Medicine in collaboration with Shenzhen No.3 People's Hospital and contains 632 lung images. Half of the images display tuberculosis infections and half do not.

![Image of Diseased and Healthy Lungs](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/LungSamplePictures.PNG)

My target variable is a binary class where 1 indicates the presence of pneumonia and 0 indicates healthy lungs. I have a set of 662 images of lungs (half healthy and half unhealthy) along with their corresponding labels. Each image is approximately 2900x2900, but the sizes can vary so they needed to be resized to a standard size. Additionally, I decided to convert the images from RGB to grayscale in order to reduce the dimensionality of the data. This transformation is especially appropriate since x-rays are black-and-white images to begin with. The data is clean and has been properly labeled by physicians. No normalization is necessary as the numeric values the pixels can take have the exact same range [0-256].

I used logistic regression as my baseline model of performance. Logistic regression is simple and could work well if the data is linearly separable. The other model I used is a random forest model, which will predict better if the decision boundary is more complex and non-linear. Each image, even after size reduction and conversion to grayscale, is very complex. Fitting random forest models for each fold could quickly become time-consuming and computationally intensive. Consequently, I felt it was appropriate to evaluate whether my model overfit using the training and test data alone rather than using cross-validation.

The random forest model performed fairly well overall, with a test set accuracy of 78 % and AUC of .827. This performance isn't terrible given the classes were equally balanced. However, the baseline logistic regression model was able to achieve a higher AUC of .897. This is likely due to the fact that there is a lot of noise in the image data. Some factors that could have contributed to this noise were that the images were resized from different original sizes, images were taken on patients who "filled up" the x-ray machine differently, and images were taken on people of different genders. Potential improvements to the modeling could include more complex image processing and feature extraction. Images could be cropped to eliminate blank space, features could be generated from 'influential' areas (in the center of the lungs). or one could apply techniques like PCA or Canny Edge Detection to pre-process the data. 

![ROC Curve](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/ROC_Curve.PNG)

![Confusion Matrix](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/ConfusionMatrix.PNG)



Different models may have also been able to fit the data better. It is possible that the flexibility of the random forest ended up being its downfall- it may have overfit due to high-dimensionality. Bagging is another approach that could reduce the correlation between individual trees and address this problem. I could also use convolutional neural networks, which implictly perform a form of feature extraction, as a modeling tool. However, I deliberately avoided using more complex techniques to try to see what kind of performance I could achieve with lower computational expenditure. 



**Citations:**

Jaeger S, Karargyris A, Candemir S, Folio L, Siegelman J, Callaghan F, Xue Z, Palaniappan K, Singh RK, Antani S, Thoma G, Wang YX, Lu PX, McDonald CJ.  Automatic tuberculosis screening using chest radiographs. IEEE Trans Med Imaging. 2014 Feb;33(2):233-45. doi: 10.1109/TMI.2013.2284099. PMID: 24108713

Candemir S, Jaeger S, Palaniappan K, Musco JP, Singh RK, Xue Z, Karargyris A, Antani S, Thoma G, McDonald CJ. Lung segmentation in chest radiographs using anatomical atlases with nonrigid registration. IEEE Trans Med Imaging. 2014 Feb;33(2):577-90. doi: 10.1109/TMI.2013.2290491. PMID: 24239990
