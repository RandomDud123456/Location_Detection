## Data Preprocessing

* **Batch Resize:** Original images resized to 224×224 with LANCZOS filtering for consistency.
* **Clean Coordinates:** Filter latitude (200k–230k) and longitude (140k–150k) ranges to remove geographic outliers.
* **CSV Management:** Save cleaned train/val metadata to new CSVs for reproducibility. Here we cleaned the files we got from Region_ID prediction which has the predicted Region_IDs and we cleaned the data to remove anomalies.
* **Transform Pipeline:** Use Hugging Face’s `ViTImageProcessor` to normalize and convert images to tensors.
* **Dataset Loader:** Custom `GeoDataset` returns `(image_tensor, region_id, coords)` for each sample.

## Feature Extraction

* **Pre-trained ViT Backbone:** Load `google/vit-base-patch16-224-in21k`, freeze weights.
* **CLS Embedding:** Extract 768-D \[CLS] token from last hidden state as image descriptor.
* **Efficient Loop:** Batched extraction with `torch.no_grad()`, progress via `tqdm`.
* **Data Packing:** Concatenate features, region labels, and true coords; save as `TensorDataset`.
* **Avoid Redundancy:** Single-pass feature extraction decouples vision and regression.

## Regression Model

* **Hybrid Input:** Concatenate 768-D features with one-hot region vector (length = num\_regions).
* **FC Stack:** Four linear layers 768+R→512→256→128→64, each followed by BatchNorm + ReLU.
* **Output Layer:** Linear regressor maps 64→2 for latitude and longitude.
* **Lightweight:** Small MLP ensures fast convergence on top of precomputed features.

## Training & Evaluation

* **Loss & Optimizer:** L1 Loss, SGD (lr=1e-3, momentum=0.9, weight\_decay=1e-4).
* **Scheduler & Early Stop:** `ReduceLROnPlateau` (patience=5) and early stopping after 10 epochs with no improvement.
* **Metrics:** Track train loss and val MAE; compute final MAE/MSE and optionally export predictions to CSV.
* **Reproducibility:** Fixed seeds, GPU support, and model checkpointing for best val performance.
* **Visualization:** Plot training vs. validation curves post-training using Matplotlib.

## Model Link

https://drive.google.com/file/d/1yHdzpiWyQN-NYXzehCcdncx091eu8w5G/view?usp=sharing