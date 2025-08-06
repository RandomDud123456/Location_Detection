# Location Detection System

This project involves a multi-stage system designed to predict the **Region ID**, **Latitude**, **Longitude**, and **Camera Angle** for a given image taken on the IIIT-H campus.

## Overview

Using a large dataset of campus images (collected by students including me), the system was developed in three stages:

1. **Region ID Classifier**  
   Predicts which of the 15 campus zones the image was captured in.

2. **Latitude and Longitude Regressor**  
   Trained conditionally based on the predicted Region ID to more precisely localize the image within the corresponding region.

3. **Camera Angle Predictor**  
   Estimates the direction (in degrees) the camera was facing when the image was taken.

## Preprocessing

- **Latitude and Longitude** values were scaled for improved model convergence.
- **Region ID** and **Angle** values were used as-is.

## Evaluation Metrics

| Task                      | Metric                     | Value         |
|---------------------------|----------------------------|---------------|
| Region ID Classification | Accuracy                   | `0.970189701` |
| Latitude/Longitude        | Mean Squared Error (MSE)   | `58219.8947037` |
| Camera Angle              | 1 / Mean Absolute Angular Error (1/MAAE) | `0.0384695` |

## Key Highlights

- A hierarchical design that improves accuracy by localizing the broader region first.
- Separate models for each sub-task ensure modularity and interpretability.
- Evaluation based on standard regression and classification metrics.




### The codebase is in the Location_Detection directory
