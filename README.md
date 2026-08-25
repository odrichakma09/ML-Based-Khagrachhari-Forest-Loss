# ML-Based-Khagrachhari-Forest-Loss
Machine learning-based forest loss prediction in Khagrachhari, Bangladesh using geospatial and environmental data.



# Khagrachhari-Forest-Loss-ML

Machine learning-based forest loss prediction in Khagrachhari, Bangladesh using geospatial and environmental predictors.

# Machine Learning-Based Forest Loss Prediction

## Khagrachhari, Bangladesh

## 1. Overview

This project explores the application of machine learning and geospatial data to investigate forest-loss patterns in Khagrachhari, Bangladesh.

The workflow integrates remotely sensed, terrain, climatic, vegetation, soil-moisture, forest-cover, and land-use/land-cover datasets with Python-based geospatial processing and machine learning.

The project covers raster preprocessing and alignment, data integration, exploratory data analysis, feature engineering, Random Forest classification, model evaluation, feature importance analysis, spatial prediction, and GIS-based visualization.

The final machine learning workflow uses DEM, Slope, Aspect, Rainfall, NDVI, Temperature, Soil Moisture, Forest Cover, and LULC-related variables to predict a binary forest-loss target.

---

## 2. Objectives

The main objectives of this project are to:

* Prepare and integrate geospatial and environmental datasets for machine learning.
* Align multiple raster datasets to a common spatial reference.
* Examine the spatial and statistical characteristics of environmental predictors.
* Perform exploratory data analysis of the integrated dataset.
* Engineer suitable features for machine learning.
* Transform the continuous Aspect variable using sine and cosine representations.
* Develop a binary Forest Loss target variable.
* Train a baseline Random Forest classification model.
* Evaluate model performance using classification metrics.
* Examine feature importance to identify the variables contributing most strongly to model predictions.
* Generate spatial forest-loss predictions.
* Visualize the resulting prediction output in a GIS environment.

---

## 3. Study Area

The study focuses on Khagrachhari District in the Chittagong Hill Tracts of southeastern Bangladesh.

The region is characterized by hilly terrain, forest ecosystems, diverse land-use patterns, and environmental pressures that make it relevant for investigating spatial patterns of forest loss.

The Khagrachhari boundary was used as the spatial extent for preparing and integrating the geospatial predictor datasets.

---

## 4. Data & Variables

The machine learning dataset was constructed by integrating multiple geospatial and environmental raster datasets.

### Predictor Variables

| Variable          | Description                            | Role      |
| ----------------- | -------------------------------------- | --------- |
| DEM               | Digital Elevation Model                | Predictor |
| Slope             | Terrain slope derived from DEM         | Predictor |
| Aspect            | Terrain orientation derived from DEM   | Predictor |
| Rainfall          | Mean rainfall                          | Predictor |
| NDVI              | Normalized Difference Vegetation Index | Predictor |
| Temperature       | Mean near-surface temperature          | Predictor |
| Soil Moisture     | Soil moisture conditions               | Predictor |
| Forest Cover 2000 | Percentage tree cover in 2000          | Predictor |
| LULC              | Land Use/Land Cover                    | Predictor |

### Target Variable

| Variable           | Description                  | Role   |
| ------------------ | ---------------------------- | ------ |
| Forest_loss_binary | Binary forest-loss indicator | Target |

The Forest Loss variable was transformed into a binary classification target:

* `0` = No forest loss
* `1` = Forest loss

### Feature Engineering

Because Aspect is a circular variable ranging from approximately 0° to 360°, it was transformed into two continuous variables:

* `Aspect_sin`
* `Aspect_cos`

The final modelling dataset therefore contained 10 predictor variables.

---

## 5. Data Sources & Processing

The geospatial datasets were prepared primarily using Google Earth Engine and subsequently processed in Python.

The workflow included:

1. Defining the Khagrachhari study boundary.
2. Preparing terrain variables from the DEM.
3. Processing rainfall data.
4. Calculating multi-year mean NDVI.
5. Processing temperature and soil-moisture datasets.
6. Preparing forest-loss and historical forest-cover data.
7. Preparing LULC data.
8. Exporting raster datasets from Google Earth Engine.
9. Checking CRS, spatial extent, resolution, shape, and transform.
10. Aligning datasets to a common reference grid.
11. Identifying common valid pixels across the predictor variables.
12. Converting aligned raster values into a tabular machine-learning dataset.

The final integrated dataset contained approximately 3.61 million common valid pixels.

---

## 6. Exploratory Data Analysis

Exploratory data analysis was performed to understand the distributions and characteristics of the predictor variables and target class.

The analysis included:

* Missing-value assessment
* Data-type verification
* Unique-value analysis
* Minimum and maximum values
* Target-class distribution
* Aspect distribution
* LULC class distribution
* Examination of environmental variables
* Feature relationships and distributions

The Forest Loss target contained a substantial class imbalance, with considerably more observations in the no-loss class than the forest-loss class.

---

## 7. Feature Engineering

Feature engineering was performed before model training.

### Aspect Transformation

Aspect was converted into two circular representations:

* `Aspect_sin`
* `Aspect_cos`

This prevents the artificial discontinuity between 0° and 360° from being treated as a large numerical difference.

### Forest Loss Target

The original forest-loss information was converted into a binary classification target:

```text
0 → No forest loss
1 → Forest loss
```

The final modelling dataset contained:

```text
DEM
Slope
Aspect_sin
Aspect_cos
Rainfall
NDVI
Temperature
Soil Moisture
Forest Cover 2000
LULC
```

---

## 8. Machine Learning

A Random Forest classifier was developed as the baseline machine learning model.

The dataset was divided into training and testing subsets before model training.

### Model

**Algorithm:** Random Forest Classifier

The model was trained using the environmental and geospatial predictor variables to classify pixels according to the binary Forest Loss target.

Random Forest was selected as a baseline because it can model nonlinear relationships between environmental variables and the target and can provide feature-importance estimates.

---

## 9. Model Evaluation

The model was evaluated using:

* Accuracy
* Precision
* Recall
* F1-score
* Confusion Matrix

Because the Forest Loss classes were imbalanced, accuracy was not considered sufficient as the only evaluation metric.

The baseline model produced an overall accuracy of approximately **77.6%** on the evaluated test sample.

### Classification Performance

| Class              | Precision | Recall | F1-score |
| ------------------ | --------: | -----: | -------: |
| 0 — No forest loss |      0.80 |   0.94 |     0.86 |
| 1 — Forest loss    |      0.61 |   0.30 |     0.40 |

The model performed considerably better at identifying the majority no-loss class than the forest-loss class.

This indicates that the current model has limitations in detecting the minority forest-loss class and should be considered a baseline exploratory model rather than a fully validated operational prediction system.

---

## 10. Feature Importance

Random Forest feature importance was used to examine the relative contribution of the predictor variables within the trained model.

| Rank | Feature           | Importance |
| ---- | ----------------- | ---------: |
| 1    | NDVI              |     0.1687 |
| 2    | DEM               |     0.1395 |
| 3    | Slope             |     0.1361 |
| 4    | Forest Cover 2000 |     0.1342 |
| 5    | Aspect_sin        |     0.1173 |
| 6    | Aspect_cos        |     0.1165 |
| 7    | Rainfall          |     0.0721 |
| 8    | Temperature       |     0.0542 |
| 9    | Soil Moisture     |     0.0513 |
| 10   | LULC              |     0.0101 |

NDVI had the highest feature-importance score, followed by DEM, Slope, and Forest Cover 2000.

These values represent the contribution of variables within the trained Random Forest model and should not be interpreted as evidence of causal relationships.

---

## 11. Spatial Prediction

The trained Random Forest model was applied to the prepared geospatial predictor dataset to generate spatial predictions of forest-loss occurrence.

The model predictions were subsequently converted back into a spatial raster format and visualized using GIS software.

The final spatial output was prepared for visualization in QGIS.

---

## 12. Results

The project produced:

* An integrated geospatial machine-learning dataset.
* A trained Random Forest classification model.
* Model evaluation metrics.
* Feature-importance results.
* A spatial forest-loss prediction output.
* A final GIS visualization of the model prediction.

The results demonstrate the potential of combining remote sensing, environmental variables, GIS, and machine learning to investigate spatial patterns of forest loss in Khagrachhari.

However, the relatively low recall for the forest-loss class indicates that additional modelling and validation are required before the approach can be considered a reliable predictive system.

---

## 13. Limitations

### 1. Class Imbalance

The Forest Loss target contained substantially more no-loss observations than forest-loss observations.

This imbalance affected the model's ability to identify the minority forest-loss class.

### 2. Model Performance

Although the evaluated accuracy was approximately 77.6%, recall for the forest-loss class was only 0.30.

Therefore, the current model should be treated as a baseline exploratory model rather than a validated operational forest-loss prediction system.

### 3. Spatial Validation

The current workflow used a conventional train/test split. Future work should investigate spatially structured validation to better assess how well the model generalizes geographically.

### 4. Predictor Representation

The current model uses aggregated environmental variables. Future research could incorporate more detailed temporal information from satellite imagery and environmental datasets.

### 5. Feature Importance

Random Forest feature-importance scores indicate model contribution but do not establish causal relationships.

---

## 14. Future Work

Potential future development includes:

* Addressing class imbalance using appropriate resampling or class-weighting strategies.
* Comparing Random Forest with gradient-boosting algorithms.
* Performing hyperparameter optimization.
* Investigating spatial cross-validation.
* Incorporating multi-temporal satellite imagery.
* Adding additional environmental and socioeconomic predictors.
* Exploring explainable AI methods such as SHAP.
* Comparing classical machine learning with deep-learning approaches.
* Developing remote-sensing-based forest-loss detection using Sentinel imagery.
* Exploring CNN-based and segmentation-based GeoAI approaches.
* Developing an interactive web-based geospatial visualization or prediction application.

---

## 15. Tools & Technologies

### Geospatial & Remote Sensing

* Google Earth Engine
* QGIS
* ArcGIS
* GeoTIFF raster datasets
* Rasterio

### Programming & Data Analysis

* Python
* NumPy
* Pandas
* Matplotlib
* Seaborn
* Google Colab

### Machine Learning

* Scikit-learn
* Random Forest
* Classification metrics
* Feature importance analysis

### Visualization & Portfolio

* QGIS
* GitHub

---

## 16. Project Structure

```text
Khagrachhari-Forest-Loss-ML/
│
├── README.md
│
├── data/
│   └── README.md
│
├── gee/
│   ├── Khagrachari_GEE_Preprocessing.js
│   └── README.md
│
├── notebooks/
│   ├── Khagrachari_Forest_Loss_ML.ipynb
│   └── README.md
│
├── maps/
│   ├── README.md
│   ├── study_area.png
│   ├── prediction_map.png
│   └── final_map.png
│
├── figures/
│   ├── README.md
│   ├── class_distribution.png
│   ├── confusion_matrix.png
│   └── feature_importance.png
│
├── models/
│   ├── README.md
│   └── random_forest_model.pkl
│
├── docs/
│   ├── README.md
│   └── methodology.md
│
├── requirements.txt
│
└── LICENSE
```

### data

Contains documentation describing the geospatial and environmental datasets used in the project.

Large raster datasets are not included directly in the repository. Data sources and processing information are documented separately.

### gee

Contains the Google Earth Engine JavaScript workflow used to prepare, process, visualize, and export the geospatial datasets.

### notebooks

Contains the main Python/Google Colab notebook documenting data integration, EDA, feature engineering, Random Forest modelling, evaluation, feature importance, and spatial prediction.

### maps

Contains selected spatial outputs and final GIS visualizations generated during the project.

### figures

Contains analytical and model-evaluation figures, including class-distribution plots, confusion matrices, and feature-importance visualizations.

### models

Contains the trained machine-learning model and supporting documentation required to understand the model artifact.

### docs

Contains supplementary methodological documentation and notes describing the overall geospatial machine-learning workflow.



## 17. Reproducibility

The project is organized to document the major stages of the workflow from geospatial data preparation through machine learning and spatial visualization.

The GEE scripts, Python notebook, methodological documentation, model outputs, and selected visualizations are provided to make the workflow easier to understand and reproduce.

Large geospatial raster datasets are excluded from the repository due to file size considerations.



## 18. Project Status

**Status: Completed — Baseline Geospatial Machine Learning Project**

The current version represents a completed baseline workflow. Future development will focus on improving spatial validation, minority-class detection, temporal remote-sensing integration, and advanced GeoAI approaches.
