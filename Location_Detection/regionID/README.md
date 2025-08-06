## Data Preprocessing 

1. **Label Adjustment**: Original Region\_ID values were decremented by 1 to align zero-based indexing.
2. **Train/Val Split**: Separate CSV files (`labels_train_new.csv`, `labels_val_new.csv`) link filenames to adjusted labels.
3. **Transforms (Training)**:

   * RandomResizedCrop(224) for scale invariance
   * RandomHorizontalFlip & ColorJitter for photometric variation
   * RandAugment (2 ops, mag=9) for stronger augmentation
   * Normalize with ImageNet mean/std
4. **Transforms (Validation/Test)**:

   * Resize(256) + CenterCrop(224)
   * Normalize with ImageNet mean/std

## Model Architecture & Initialization 

1. **Backbone**: `convnext_base` CNN pretrained on ImageNet, providing robust feature extraction.
2. **Classifier Head**: Replaced final Linear layer to output 15 region classes.
3. **Device**: Automatically selects CUDA if available, else CPU.
4. **Criterion**: CrossEntropyLoss with label smoothing (0.1) to mitigate overconfidence.
5. **Optimizer**: AdamW with weight decay (1e-2) for regularization.
6. **LR Scheduler**: Custom cosine schedule with linear warmup over 5 epochs.
7. **Early Stopping**: Monitors validation loss with patience of 5 epochs, saving the best checkpoint (`area.pth`).

## Training Enhancements & Innovations

1. **Mixup Augmentation**: On-the-fly mixup (alpha=0.2) to improve robustness and calibration.
2. **Gradient Accumulation**: Steps=2 to effectively increase batch size under GPU memory constraints.
3. **Differentiated Learning Rates**: Lower LR for backbone (1e-5) vs. classifier (1e-4) for stable fine-tuning.
4. **Progress Logging**: `tqdm` progress bars with live loss/accuracy metrics.

## Prediction & Post-processing 

1. **Train/Val Predictions**: Load best checkpoint, run inference, save (`area_predictions_{split}.csv`).
2. **Index Adjustment**: Region\_ID predictions incremented back by 1 for submission format.
3. **Test Predictions**: Continuous ID offset (start at 369) to avoid ID collisions; append to CSV.
4. **Merging**: Combined predicted labels with original metadata (filename, lat, lon) for uasge in latitude and longitude prediction, i.e. using predicted Region IDs along with image for latitude and longitude prediction.

## Model Link

https://drive.google.com/file/d/1775OhDAiAz7SUdWX_cGr7BgJZc-C7B1q/view?usp=sharing

