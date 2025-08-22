# Deforestation Detection AI 🌳🛰️

## Overview

This project is an end-to-end pipeline for building and evaluating an AI model that detects deforestation from satellite imagery. The system automatically gathers labeled data for "affected" (deforested) and "unaffected" (non-deforested) regions, trains a custom Convolutional Neural Network (CNN) on this data, and provides a detailed evaluation of the model's performance.

The final model is trained on **NDVI (Normalized Difference Vegetation Index)** images, which provide a strong, clear signal for vegetation health, allowing for accurate detection of land cover changes.

## Key Features

-   **Automated Data Collection:** Scripts to programmatically download thousands of labeled bounding boxes for both deforested and non-deforested areas using reliable, public datasets.
-   **Satellite Imagery Integration:** Fetches real NDVI satellite images for each bounding box using the **Sentinel Hub API**.
-   **AI Model Training:** Builds and trains a custom CNN specifically designed to classify NDVI images.
-   **Model Evaluation:** Includes a script to test the trained model on an unseen, labeled dataset to get a true measure of its real-world performance.
-   **Performance Visualization:** Generates a polished and detailed **confusion matrix** heatmap to visualize the model's accuracy, precision, recall, and F1-score.

---

## ⚙️ Project Workflow

The project is structured as a series of Python scripts that should be run in order:

1.  **Data Generation:**
    * `get_affected_1000.py`: Downloads 1000+ bounding boxes from known deforestation zones (WWF Deforestation Fronts).
    * `get_unaffected_1000.py`: Downloads 1000+ bounding boxes from non-deforested regions (Natural Earth Urban Areas).
2.  **Dataset Combination:**
    * `combine_and_label.py`: Merges the two datasets into a single `labeled_deforestation_dataset.csv`, adds the `isAffected` label, and shuffles the data.
3.  **Model Training:**
    * `train_model_ndvi.py`: Reads the combined CSV, fetches the corresponding NDVI satellite image for each bounding box, and trains the custom CNN. The final model is saved as `deforestation_model_custom_ndvi.h5`.
4.  **Model Testing & Evaluation:**
    * `test_model_on_csv.py`: Tests the saved model on a separate `test_dataset.csv`.
    * `plot_confusion_matrix.py`: Takes the results from the test script and generates a high-quality visualization of the model's performance.

---

## 🛠️ Setup & Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/akshat2474/CarbonEye_AI
    ```

2.  **Create a virtual environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install the required packages:**
    ```bash
    pip install -r requirements.txt
    ```
    The `requirements.txt` file should contain:
    ```text
    tensorflow
    pandas
    scikit-learn
    Pillow
    requests
    sentinelhub-py
    eo-learn
    matplotlib
    seaborn
    tqdm
    ```

4.  **Configure Sentinel Hub Credentials:**
    Open the training and testing scripts and replace the placeholder values for `CLIENT_ID` and `CLIENT_SECRET` with your actual Sentinel Hub credentials.

---

## Usage

Run the scripts from your terminal in the following order.

1.  **Generate the dataset:**
    ```bash
    python get_affected_1000.py
    python get_unaffected_1000.py
    python combine_and_label.py
    ```
    This will create the final `labeled_deforestation_dataset.csv`.

2.  **Train the AI model:**
    * Make sure your Sentinel Hub credentials are set in the script.
    * This step will take time as it downloads thousands of images.
    ```bash
    python train_model_ndvi.py
    ```
    This will create the `deforestation_model_custom_ndvi.h5` file.

3.  **Test the model on your test data:**
    * Make sure your `test_dataset.csv` is in the project folder.
    * Make sure your Sentinel Hub credentials are set in the script.
    ```bash
    python test_model_on_csv.py
    ```

4.  **Visualize the results:**
    * Update the script with the final numbers from the test run.
    ```bash
    python plot_confusion_matrix.py
    ```

---

## Model Performance

The final model, trained on a large, balanced NDVI dataset, achieved the following performance on an unseen test set:

-   **Accuracy:** 93.75%
-   **Precision:** 87.88%
-   **Recall:** 72.50%
-   **F1-Score:** 79.45%
