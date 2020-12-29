
# Pneumonia Detection From Chest X Rays
Index:
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
- The training dataset consists of 2290 images, where 50% correspond to positive pneumonia studies.
- The validation dataset consist of 1430 images, where 20% of them are from Pneumonia studies.
- Both datasets are slight imbalance between gender, having more male studies.
- The age distribution for both datasets go from 10 to 80.

| Training | Gender Distribution | Age Distribution |
|----------|--------|-----|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/training_dataset.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/training_dataset_gender.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/training_age_distribution.png)|

**Description of Validation Dataset:** 
| Validation | Gender Distribution | Age Distribution |
|----------|--------|-----|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/validation_dataset.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/validation_gender_distribution.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/validation_age_distribution.png)|

| Random sample of images after applying data augmentation |
| ---- |
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/data_augmentation.png)|

### Ground Truth
- The NIH Chest X-ray dataset uses NLP to obtain the labels which can lead to not having the real disease.
- It is highly imbalanced containing more than 50% of non pneumonia cases as it is a more general dataset.
- Many patients in this dataset have more than one disease comorbid with Pneumonia. This could impact the algorithm clinical performance.

| NIH distribution | Diseases comorbid with Pneumonia |
|------------------|------------------|
|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/NIH_distribution.png)|![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/diseases_comorbid_with_pneumonia.png)|

### FDA Validation Plan

**Patient Population Description for FDA Validation Dataset:**
- The population used for the dataset are men and women with ages between 10 and 80 with no previous history of Pneumonia. 
- The image type to use is `DX` and should only be used with chest images either `AP` or `PA`.

**Ground Truth Acquisition Methodology:**
Since identifying Pneumonia is difficult for radiologists, we will use silver standard as the Ground Truth.

**Algorithm Performance Standard:**
- Since our algorithm is used for screening studies we will focus on *recall*
- We want to achieve a high recall, since our algorithm is used for screening studies, in our case we got a 0.8.
- The F1 score of our algorithm was 0.4, this score is slightly higher to the average of radiologists `0.387`  according to the [CheXNet](https://arxiv.org/pdf/1711.05225.pdf) paper.


### Results

![Image](https://github.com/jb-apps/Udacity-Pneumonia-Detection-From-Chest-X-Rays/blob/main/assets/Testing.png)