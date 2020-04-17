
# CISC 499: Machine Learning for Detecting Lesions in Focal Epilepsy

### Project Abstract:
Identifying areas of abnormality in individuals with focal epilepsy is fundamental to the diagnosis and treatment of the condition. However, in a third of patients with focal epilepsy, MRI brain scans appear to be normal (MRI-negative) because the imaging technology could not clearly detect any abnormal areas. The objective of this research is to use modern machine learning techniques in order to locate areas of abnormality on patients with focal epilepsy on a per voxel basis by comparing them with healthy controls. Multimodal brain MR images were used to train a classifier from 62 healthy control subjects and 44 MRI-positive patients with focal epilepsy. Probability maps were generated from test subjects in order to visualize predictions from the classifier and calculate overlaps in MRI-positive patients. The model predicted voxel abnormality with an area under the receiver operating characteristic curve of 0.896. In positive outcomes, the model could predict true positives with a very high probability, however struggled when presented with areas of the brain it hadn't been exposed to during training with similar intensities and often misclassified them as positive, although with lower probabilities. This study suggested that voxel-based classification can be possible with favourable results, however the approach would likely benefit from modifications to the training step, as well as choosing a model that takes the relationships of neighbouring voxels into consideration. After making adjustments to the model, moving forward, this research may serve as a solid first step in training a model that can uncover hidden areas of abnormality in MRI-negative patients.

## Project steps:

**1. Convert MR images to Z-maps** - *zscoring.ipynb*
* All of the MRI modalities for all patients in both the control and discrete groups were converted into Z-maps. 
* Z-maps were calculated by finding the Z-score of each individual voxel of each scan using the respective modality's control group mean and standard deviation. 

**2. Apply lesion masks to Z-maps** - *lesions.ipynb*
* Discrete lesion masks were applied to the respective discrete patients' Z-maps to extract those abnormal voxels
* For the control group, lesions were chosen at random to extract healthy voxels

**3. Feature extraction** - *features.ipynb*
* DataFrames were created for each patient as SVM input. 
* DataFrames were Nx7 shape where N = number of voxels in the patient lesion.
* The first 5 columns in the DataFrame were the modalities, so the rows were populated with each voxel value on the Z-maps at each modality
* The last 2 columns were target_name (control, discrete) and a label (0, 1)

**4. Train SVM** - *svm.ipynb*
* The patient DataFrames were split into test and train data
* 5-fold cross-validation was performed on the model
* Performance metrics such as AUROC, accuracy, sensitivity, specificity were calculated

**5. Assess accuracy & generate probability maps** - *testing.ipynb*
* The model was applied to entire individual test subject scans.
* From these predictions, probability maps were generated for visualization purposes
* Dice score (overlap measure) was calculated to assess the predictions

## This project was built with:
* [Scikit-learn](https://scikit-learn.org/stable/index.html) for training and testing the SVM model and assessing accuracy through ROC curves, classification reports, confusion matrices, etc.
* [SimpleITK](https://scikit-learn.org/stable/index.html) for converting the MRI nifti files into SITK Image objects and NumPy arrays to perform direct mathematical operations on them (multiplying lesion masks, segmentations, feature extraction)
* [3D Slicer](https://www.slicer.org/) for visualization purposes such as viewing MR images, overlaying lesion masks and viewing the probability maps
* [Jupyter](https://jupyter.org/) as the development environment 

Other helpful open source libraries used for implementation:
* [NumPy](https://numpy.org/) - represent MRIs as 3D arrays and other helpful BIFs
* [pandas](https://pandas.pydata.org/) - creating DataFrames with features for SVM input
* [Matplotlib](https://matplotlib.org/) - plotting graphs
* [Joblib](https://joblib.readthedocs.io/en/latest/#) - saving SVM model

## Acknowledgements
* Thank you to Dr. Parvin Mousavi and Dr. Gavin Winston for supervising this project
* Thank you to Jonah Isen for assisting with this project 
