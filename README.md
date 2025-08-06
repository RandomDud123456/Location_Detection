# Location_Detection

Using a large dataset of images collected across the IIIT-H campus, I built a multi-stage system to predict the Region ID, Latitude, Longitude, and Camera Angle for a given image.

The system consists of three models:

    A Region ID classifier that predicts which of the 15 campus zones the image was taken in.

    A Latitude and Longitude regressor, trained conditioned on the predicted Region ID, to localize the image more precisely within the predicted region.

    An Angle predictor that estimates the direction (in degrees) the camera was facing when the image was captured.

The latitude and longitude values were appropriately scaled for model training, while the Region ID and angle values remained unchanged.

The system was evaluated using:

    Accuracy (up to 6 decimal places) for Region ID.

    Mean Squared Error (MSE) for Latitude, Longitude, and Angle.

This staged approach allowed the model to first localize the broader region before refining the exact coordinates, improving both accuracy and interpretability.

The codebase is in the Location_Detection directory
