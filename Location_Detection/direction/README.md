## Data Preprocessing and Augmentation

1. **Image Resizing & Cropping**

   * Train images are resized to **256×256** pixels, then randomly cropped to **224×224** for spatial consistency and translational augmentation.
2. **Normalization**

   * Apply ImageNet mean **(\[0.485, 0.456, 0.406])** and standard deviation **(\[0.229, 0.224, 0.225])** to all inputs.
3. **Color Jitter & Grayscale**

   * Brightness, contrast, and saturation jitter (±20%), plus a 10% chance of random grayscale, to improve robustness against lighting variations.
4. **Gaussian Blur & Sharpness**

   * 10% chance to apply Gaussian blur and 10% chance to adjust sharpness, enhancing invariance to focus and blur.
5. **Random Affine Shear**

   * 20% chance of small affine shear and translation to simulate minor viewpoint changes.

---

## Model Architecture

### Backbone

* **ResNet-50** pre-trained on ImageNet serves as a feature extractor.
* The original fully connected layer is removed and replaced by an identity mapping to output raw feature vectors.

### Regressor Head

* **LayerNorm** followed by three residual linear blocks with dimensions **2048→1024→512→256**.
* Each block uses **GELU** activations, **dropout**, and residual connections.
* A final linear layer projects features to a **2D sine-cosine representation** of the angle.
* **L2-normalization** ensures valid sine-cosine pairs.

---

## Innovative Ideas

* **Sine-Cosine Encoding**: Represent angles as continuous 2D vectors to avoid discontinuities at 0°/360°, enhancing regression stability.
* **Custom Angular Loss**: Combines cosine similarity, mean absolute angular difference, and a circumference-aware term to penalize large circular errors.
* **Adaptive Training Stability**: Uses mixed-precision training with automatic casting and gradient scaling for faster, stable convergence.
* **Cosine Annealing Scheduler**: Implements a cosine decay learning rate schedule for smooth convergence without manual decay steps.



## Model Link

https://drive.google.com/file/d/1_19EJGt3BGXRUpL4pIUhYTwx1qFD2X_g/view?usp=sharing

---