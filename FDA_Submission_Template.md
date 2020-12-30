
# FDA  Submission

**Jonathan Benavides Vallejo**

**Classifier for Assisting in Pneumonia**

## Algorithm Description 

### 1. General Information

**Intended Use Statement:** 
For assisting radiologists into detecting Pneumonia on chest X-Rays.

**Indications for Use:**
* Use in chest X-Rays screening studies.
* Use in men and women with ages between 10 and 80.
* Use with images PA and AP.


**Device Limitations:**
* The classifier is not tested on patients with previous history of Pneumonia.
* Runs slowly without a GPU.


**Clinical Impact of Performance:**

As we are more interested in reducing the number of patients sent home, we have to reduce the False Negatives. Hence, we need to focus on maximizing the recall, and also because our algorithm is designed to help in screening studies. 

False Positives does not have a big impact on a patients, it is okay to send a healthy patient to a new screening.


### 2. Algorithm Design and Function
|Algorithm|
|----|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/Algorithm.png)|

**DICOM Checking Steps:**
We check for 3 parameters:
- Image type (Modality) is a Digital X-ray (DX)
- Image body position is Chest.
- Image Position is either Anteior-Posterior(AP) or Posterior-Anteior PA.

If all these conditions are met, then we return:
- Image pixel data.
- Image mean.
- Image standard deviation.


**Preprocessing Steps:**
- We first standarize the image using the mean and standard deviation.
- We resize the image using the image size used for training.

    
**CNN Architecture:**

VGG16 CNN was used for transfer learning. All but the last block were freezed.
We got better results by fine tuning the last block which has 3 convolutional layers and a MaxPooling2D.
We've also added 3 fully-connected layers of 1024, 512 and 256 units using `relu` as the activation function,
We've also added a 20% dropout to each layer.

Finally we've added a fully-connected layer with one unit using `sigmoid` as the activation function. The reason behind not returning the `logits` is because we are using `binary_crossentropy` as the loss function and according to the [documentation](https://keras.io/api/losses/probabilistic_losses/#binary_crossentropy-function) by default it doesn't expect logits.
* **loss** = 'binary_crossentropy'
* **optimizer** = 'adam'
* **metrics** = ['binary_accuracy']

|Training Loss & Accuracy on Dataset|Precision-Recall|AUC|
|------|------|------|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/download.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/download-2.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/download-1.png)|


### 3. Algorithm Training

**Parameters:**
* Types of augmentation used during training
	* Rescale = 1. / 255.0
	* Horizontal flip = True
	* Vertical flip = False
	* height_shift_range = 0.1
	* width_shift_range = 0.1
	* rotation_range = 20
	* shear_range = 0.1
	* zoom_range = 0.1
* Batch size for training = 64
* Batch size for validation = 18
* Optimizer learning rate = 0.0004
* Layers of pre-existing architecture that were frozen
```
input_1 (InputLayer)         (None, 224, 224, 3)       0         
block1_conv1 (Conv2D)        (None, 224, 224, 64)      1792      
block1_conv2 (Conv2D)        (None, 224, 224, 64)      36928     
block1_pool (MaxPooling2D)   (None, 112, 112, 64)      0        
block2_conv1 (Conv2D)        (None, 112, 112, 128)     73856     
block2_conv2 (Conv2D)        (None, 112, 112, 128)     147584    
block2_pool (MaxPooling2D)   (None, 56, 56, 128)       0         
block3_conv1 (Conv2D)        (None, 56, 56, 256)       295168    
block3_conv2 (Conv2D)        (None, 56, 56, 256)       590080    
block3_conv3 (Conv2D)        (None, 56, 56, 256)       590080    
block3_pool (MaxPooling2D)   (None, 28, 28, 256)       0         
block4_conv1 (Conv2D)        (None, 28, 28, 512)       1180160   
block4_conv2 (Conv2D)        (None, 28, 28, 512)       2359808   
block4_conv3 (Conv2D)        (None, 28, 28, 512)       2359808   
block4_pool (MaxPooling2D)   (None, 14, 14, 512)       0        
```
* Layers of pre-existing architecture that were fine-tuned
```
block5_conv1 (Conv2D)        (None, 14, 14, 512)       2359808
block5_conv2 (Conv2D)        (None, 14, 14, 512)       2359808
block5_conv3 (Conv2D)        (None, 14, 14, 512)       2359808   
block5_pool (MaxPooling2D)   (None, 7, 7, 512)         0
```
* Layers added to pre-existing architecture
```
Flatten()

Dense(1024, activation = 'relu')
Dropout(0.2)

Dense(512, activation = 'relu')
Dropout(0.2)
    
Dense(256, activation = 'relu')
Dropout(0.2)

Dense(1, activation = 'sigmoid')
```


**Final Threshold and Explanation:**

In clinical situations:
- **Precision** is used for confirming a diagnosis and it doesn't take **False Negatives** into account.
- **Recall** is used in screening studies and it takes **False Negatives** into account.

As our algorithm is aim for screening studies we must focus on maximizing **recall** as we want to be more confident when the test is negative. A high recall will mean **False Negatives** will be close to zero.

We've selected a threshold of `0.28` obtaining a recall of `0.78` penalising precision to `0.28`.


### 4. Databases

**Description of Training Dataset:** 

- The training dataset consists of 2290 rows with 27 features each one, we've create a balanced `pneumonia_class` column where 50% correspond to positive pneumonia studies and the rest 50% with random selected samples with no pneumonia studies.
- - The dataset is slightly imbalanced between gender, having more male studies than females.
- The age distribution goes from 10 to 80 as seen in the below image.

| Training Dataset distribution | Gender Distribution | Age Distribution |
|----------|--------|-----|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/training_dataset.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/training_dataset_gender.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/training_age_distribution.png)|
|Balanced with 50% pneumonia cases| Slightly imbalanced, with more males than females studies|More cases between 10 and 80 years|


**Description of Validation Dataset:** 
- The validation dataset consist of 1430 rows each one with 27 features, we've create an imbalanced `pneumonia_class` column where 20% correspond to positive pneumonia studies and the rest 80% of random selected samples with no pneumonia studies.
- The dataset is slightly imbalanced between gender, having more male studies than females.
- The age distribution goes from 10 to 80 as seen in the below image.

| Validation dataset distribution | Gender Distribution | Age Distribution |
|----------|--------|-----|
|![](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/validation_dataset.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/validation_gender_distribution.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/validation_age_distribution.png)|
| 20% pneumonia studies | Slightly imbalanced, with more males than females studies | More cases between 10 and 80 years|

### 5. Ground Truth

The NIH Chest X-ray Dataset is highly imbalanced in relation with patients with pneumonia. NLP-derived labels from the NIH are sub-optimal since they are more general than only the case of Pneumonia. 
One patient can have more than one disease similar to Pneumonia and many of them with more prevalence than Pneumonia in the dataset. This could negatively impact the algorithm clinical performance.

| NIH distribution | Diseases comorbid with Pneumonia |
|------------------|------------------|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/NIH_distribution.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/diseases_comorbid_with_pneumonia.png)|


### 6. FDA Validation Plan

**Patient Population Description for FDA Validation Dataset:**
Ideally we would like to obtain a dataset with chest images from `DX` studies as the body part examined, also age range, gender distribution, pneumonia, and pneumonia comorbid with other diseases.

This should be a balanced dataset with population of men and women with ages between 10 and 80 with no previous history of Pneumonia. 


**Ground Truth Acquisition Methodology:**

Since identifying Pneumonia is difficult for radiologists, we will use silver standard approach as the Ground Truth.


**Algorithm Performance Standard:**
Since our algorithm is used for screening studies we will focus on *recall*. We want to achieve a high recall, at least higher than 0.7.
The F1 score of our algorithm was 0.419, this score is slightly higher to the average of radiologists `0.387`  according to the [CheXNet](https://arxiv.org/pdf/1711.05225.pdf) paper.