# Radiant Earth Spot the Crop Hackathon

### Approach
As the hackathon only took 2 days we decided to use a relatively simple CatBoost model and spend more time on feature engineering. Multiple additional features were extracted from the Sentinel-1 time-series data.

### Temporal Features
First, only a subset of the available timesteps were used to minimize processing time. For each tile the closest available date to the 15th of each month from from April to October was used. In addition to the VV and VH signals available, additional temporal features for each pixel were added by calculating the mean and standard deviation over three consecutive months.

### Spatial Features
In order to make use of spatial information the mean and standard deviation of the 5x5 window around each pixel were calculated and used as additional features for each month. The individual pixel-level features were then aggregated over each field again using both mean and standard deviation to aggregate. This results in one feature vector per field including the aggregated values for the raw VV and VH data, the spatial and the temporal features.

### Model and Prediction
The final hyperparameters for the CatBoost model were selected using some basic hyperparameter tuning on a validation set. The fields were split into training and validation sets based on their tile to avoid having fields from the same tile in both training and validation sets as this could potentially lead to overfitting. The final model was then retrained on the full training data before the final predictions were made.

### Code
All code needed to produce the final prediction can be found in the radiant_earth_hackathon_notebook.ipynb notebook. The code was run on Google Colab and all dependencies can be found in the notebook code. All data needed for the model will be downloaded using the ML Hub API. You will have to set the os.environ['MLHUB_API_KEY'] variable to use your own API key to run the code, as the given key was just added as an example and is not active anymore.
