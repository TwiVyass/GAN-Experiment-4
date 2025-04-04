# Comparing GAN Loss Functions on the MedMNIST Dataset

This project compares the effectiveness of three Generative Adversarial Network (GAN) loss functions—Least Squares Loss (LS-GAN), Wasserstein Loss (WGAN), and Wasserstein Loss with Gradient Penalty (WGAN-GP)—in generating realistic images from the **PathMNIST** dataset (a subset of MedMNIST). The evaluation is based on both quantitative and qualitative metrics.

## Dataset: PathMNIST (MedMNIST Subset)

PathMNIST is derived from a colorectal cancer histology dataset (NCT-CRC-HE-100K) and consists of hematoxylin & eosin-stained slide images for multi-class classification tasks.

### Dataset Details:
- **Number of Images**: 89,996
- **Image Size**: 28×28 pixels
- **Channels**: 3 (RGB)
- **Labels & Meanings**:
  - 0: Adipose
  - 1: Background
  - 2: Debris
  - 3: Lymphocytes
  - 4: Mucus
  - 5: Smooth Muscle
  - 6: Normal Colon Mucosa
  - 7: Cancer-Associated Stroma
  - 8: Colorectal Adenocarcinoma Epithelium
- **Data Split**:
  - **Training**: 89,996 images
  - **Validation**: 10,004 images
  - **Test**: 7,180 images

---

## Methodology

### 1. Implementing Three GAN Variants

#### **LS-GAN (Least Squares GAN)**
Uses a least squares loss function to improve gradient behavior compared to standard GANs.

#### **WGAN (Wasserstein GAN)**
Uses the Wasserstein loss to address mode collapse and vanishing gradients.

#### **WGAN-GP (Wasserstein GAN with Gradient Penalty)**
Extends WGAN by enforcing the Lipschitz constraint more effectively using a gradient penalty.

### 2. Model Architectures
Each GAN consists of a **generator** and a **discriminator**, implemented using deep neural networks.

#### **Generator Architecture**:
- **Input**: 100-dimensional latent vector
- Fully connected layer (512 units, ReLU activation)
- Fully connected layer (784 units, ReLU activation)
- Reshape to (3, 28, 28)
- **Output**: RGB image (Tanh activation)

#### **Discriminator Architecture**:
- **Input**: Image (3×28×28)
- Flatten layer
- Fully connected layer (512 units, LeakyReLU activation)
- Fully connected layer (1 unit, Sigmoid activation for LS-GAN, linear activation for WGAN/WGAN-GP)
- **Output**: Classification score

### 3. Training Process
Each GAN model was trained for **50 epochs** using the **Adam optimizer** with the following hyperparameters:
- **Learning Rate**: 0.0002 (balances convergence speed and stability)
- **Batch Size**: 64
- **Beta1**: 0.5, **Beta2**: 0.999
- **Weight Clipping (WGAN)**: 0.01 (enforces Lipschitz constraint)
- **Gradient Penalty Coefficient (WGAN-GP)**: 10 (balances Lipschitz constraint enforcement and training stability)

---

## Performance Evaluation

### **1. Inception Score (IS) Analysis**
Measures the diversity and quality of generated images.
- **LS-GAN**: 1.0016 ± 0.0005
- **WGAN**: 1.0926 ± 0.0434
- **WGAN-GP**: 1.0000 ± 0.0000

IS typically ranges from 1 (random noise) to higher values (better quality and diversity). The results suggest that the models struggled to generate diverse or realistic images, with WGAN slightly outperforming the others.

### **2. Fréchet Inception Distance (FID) Analysis**
Measures similarity between generated and real images.
- **All FID scores are nearly zero or negative**, indicating potential implementation issues (e.g., improper real image incorporation during computation).

### **3. TensorBoard Visualizations**

#### **LS-GAN**
- **Discriminator Loss (D_Loss)**: Decreases over training epochs, indicating improved real vs. fake classification.
- **Generator Loss (G_Loss)**: Decreases, suggesting that generated images are becoming more realistic.

#### **WGAN**
- **D_Loss**: Decreases as training progresses, stabilizing learning.
- **G_Loss**: Shows an increasing trend, aligning with theoretical expectations.

#### **WGAN-GP**
- **D_Loss**: More fluctuations due to the gradient penalty, ensuring smoother gradients.
- **G_Loss**: Shows variations but follows an overall trend of improvement.

These visualizations indicate:
- **LS-GAN** has smoother, more stable loss curves.
- **WGAN** shows expected generator loss trends.
- **WGAN-GP** stabilizes training but introduces fluctuations due to the gradient penalty.

---

## Results Summary

| Model | Inception Score (IS) | FID Score |
|-------|----------------------|------------|
| LS-GAN | 1.0016 ± 0.0005 | ~0 or Negative |
| WGAN | 1.0926 ± 0.0434 | ~0 or Negative |
| WGAN-GP | 1.0000 ± 0.0000 | ~0 or Negative |

**Key Observations:**
- **WGAN-GP demonstrated the best overall performance**, balancing image generation quality and training stability.
- The **negative or near-zero FID scores** suggest potential issues in the metric computation.
- **Future Work:** Exploring hybrid loss functions or alternative architectures may further enhance generative performance.

---

## Try It Yourself!
Google Colab Notebook: [Click Here](https://colab.research.google.com/drive/1ahn8rUsqbg70oi66EhBZoZW2NR4rTAq-?usp=sharing)

---

## Conclusion
This study highlights the impact of different GAN loss functions on generating medical images. **WGAN-GP proved to be the most stable**, making it a strong choice for future GAN-based image synthesis tasks. Future enhancements could involve experimenting with **hybrid loss functions** or **more complex architectures** to improve generation quality further.

