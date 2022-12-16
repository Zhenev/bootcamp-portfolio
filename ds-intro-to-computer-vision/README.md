# A long way to precise age detection

## Data:

The dataset includes:
- The `final_files` folder with 7.6k photos;
- The `labels.csv` file with labels, with two columns: `file_name` and `real_age`.

## Goal:

Building a model to evaluate the age of a supermarket client from his / her picture taked by a on-checkout out camera.


## Libraries used:

pandas | 
matplotlib | 
keras

## Contents

- Data loading
- EDa
- Modeling
- Conclusions


## Summary

### Dataset

Our main observations at the EDA step were:
1. We had images labeled with ages 1 to 100;
2. 25% of the sample being images of people up to 20 years old, 50% of the images being of people aged 20 to 41;
3. After age 50, there were less than 50 images for each year with only one spike for the 60 years old group;
4. There were pronounced spikes in the number of images labeled 1, 25, 30, 40, 50, 60, 70, 80, and 90 years old; we should have to assume this peculiarity can be a bias of the labeling methodology (expecially in case, there were no exact dates of birth available for people on the images) and watch if it has its impact on the model;
5. We printed a batch of images to receive an overall impression of the dataset and found that the labeling has its flaws: e.g. one kid in the print out was labeled age 4, though the image showed a toddler under age 2.


### ML

At the modeling step, we applied `ResNet50`  model, 50-layer variant of ResNet. At it's end, the network has an Average Pooling layer followed by a fully connected layer having 1000 neurons (ImageNet class output). On top of it, we are experimenting with a set of fully connected layers. In the output layer we only need 1 neuron to return the predicted age value; thus, the layers between ResNet output and the model output are possible ways of reducing the number of neurons from 1000 to one.

We trained the model on 20 epochs; it resulted in MAE of 6.7 (years) for the validation set, and 5.8 (years) for the training set. The moderate difference between the two subsets can be explained by adding a more robust model to overfitting by adding dropout layers. Nevertheless, 6.7 years seems to be too much for age definition as far as alcohol is involved; thus, the model can be implemented as a notification tool only for a person on duty. Possible options are: a more precisely labeled dataset should be used, then a more sofisticated model can be developed.

## Challenges

### Data upload

Given the fact that the number of image files is rather high, we avoid reading them all at once, which would greatly consume computational resources, and build a generator with the ImageDataGenerator generator.

### Modeling
The usual challenge of a NN, is its architecture, including layers, activation functions, possibility for overfitting, and scoring:
- The choice of the ResNet (Residual Network) Architecture was driven by the fact that the training error does not increase as the depth of the neural network increases, the accuracy gains are higher as the depth increases thus producing results substantially better than previous architectures [source](https://arxiv.org/pdf/1512.03385.pdf). 
- As activation, we applied `ReLU`, which preserves positive numbers and brings all the negative ones to zero (no negative numbers for the age).
- As additional mean to reduce overfitting, we aded `Dropout` layers.
- For scoring the model, we wapplied `mae` score, given that our task is regression on a skewed distribution, and `mae` is less sensitive to the outliers and has more intuitive explanation. Note: As to the loss function, neural networks with the MSE loss function are typically trained faster than with the MAE ones.
