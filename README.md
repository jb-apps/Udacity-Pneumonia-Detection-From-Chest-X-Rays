
# Pneumonia Detection From Chest X Rays

- [Exploratory data analysis](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/EDA.ipynb)
- [Build and Training the model](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/Build%20and%20train%20model.ipynb)
- [Inference](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/Inference.ipynb)
- [FDA Submission](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/FDA_Submission_Template.md)

This project uses the [NIH Chest X-ray](https://nihcc.app.box.com/v/ChestXray-NIHCC) dataset to assist radiologists in the detection of Pneumonia.

The dataset is highly imbalanced as more than the 50% of studies are from cases with no findings.

| NIH dataset |
|-------|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/NIH_distribution.png)|

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


### Results

![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/Testing.png)