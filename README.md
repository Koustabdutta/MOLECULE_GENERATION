Here’s a **README** template for your **Molecule Generation VAE** project, which includes instructions for downloading the QM9 dataset, training a VAE model, generating molecules, and visualizing the results. It incorporates the steps from the `Molecule.ipynb` notebook.

---

# Molecule Generation VAE

This project demonstrates how to train a **Variational Autoencoder (VAE)** to generate novel molecules based on the **QM9 dataset**. The project processes the dataset using **PyTorch Geometric**, extracts valid SMILES representations, and builds a VAE model to perform molecule generation tasks.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Installation](#installation)
3. [Dataset](#dataset)
4. [Training the Model](#training-the-model)
5. [Generating New Molecules](#generating-new-molecules)
6. [Visualizations](#visualizations)
7. [Usage](#usage)
8. [License](#license)

---

## Project Overview

This project trains a **Variational Autoencoder (VAE)** to generate **molecules** represented as **SMILES strings**. It uses the **QM9 dataset**, which contains molecular information, including atomic numbers, 3D coordinates, and molecular properties (e.g., HOMO, LUMO, Gap).

The main steps in this project include:

1. **Downloading** the QM9 dataset from Kaggle using the **Kaggle API**.
2. **Extracting SMILES** from the QM9 data files.
3. **Training** a VAE model on the SMILES strings.
4. **Generating new molecules** by sampling from the learned latent space of the VAE.
5. **Visualizing** the generated molecules and training loss curves.

---

## Installation

### Prerequisites

You will need to have Python 3.6+ installed. You also need to install the following dependencies:

```bash
pip install torch torchvision torch-geometric rdkit tqdm pandas numpy matplotlib seaborn
```

Additionally, you need to install the **Kaggle API** for downloading the QM9 dataset:

```bash
pip install kaggle
```

Make sure you have a valid **Kaggle API key** (`kaggle.json`) in your `~/.kaggle/` directory (Linux/Mac) or `%USERPROFILE%\.kaggle\` directory (Windows).

---

## Dataset

The dataset used in this project is the **QM9 dataset**, which contains **267,771 molecules** along with their 3D coordinates and molecular properties. It is available on Kaggle: [QM9 - Kaggle Dataset](https://www.kaggle.com/datasets).

### Steps to Download the Dataset:

1. Ensure you have the Kaggle API key (`kaggle.json`).
2. Run the script to download the dataset directly from Kaggle:

   ```bash
   kaggle datasets download zaharch/quantum-machine-9-aka-qm9
   ```

   This will download the data to the `data/qm9_kaggle` directory.

---

## Training the Model

The model used in this project is a **Variational Autoencoder (VAE)**, which learns to represent SMILES strings of molecules in a compressed latent space. Here are the main steps:

1. **Data Preprocessing**:

   * Extracts SMILES representations from the QM9 dataset.
   * Tokenizes the SMILES strings into sequences of characters and pads them to a uniform length.

2. **Model Architecture**:

   * The VAE is implemented using a **GRU-based encoder and decoder**.
   * The encoder compresses the SMILES string into a latent space, and the decoder reconstructs the SMILES from the latent vector.

3. **Training**:

   * The VAE is trained using **Reconstruction Loss (BCE)** and **KL Divergence Loss**.

4. **Optimization**:

   * The optimizer used is **Adam** with a learning rate of `1e-3`.
   * The model is trained for `50 epochs`.

---

## Generating New Molecules

After the model is trained, you can generate new molecules by sampling from the latent space. This is done by sampling random latent vectors and using the decoder to reconstruct SMILES strings.

### Code Example:

```python
# Sample a random latent vector
random_latent = torch.randn(1, LATENT_DIM).to(device)

# Generate a new molecule from the latent vector
generated_smiles = model.sample(z=random_latent, max_len=120, temperature=0.8)

print(f"Generated SMILES: {generated_smiles[0]}")
```

---

## Visualizations

### Training Loss Curve

The training loss curve is plotted to monitor the performance of the model during training.

```python
import matplotlib.pyplot as plt

# Plot training loss curve
plt.plot(train_losses, label='Train Loss')
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.title('Training Loss')
plt.legend()
plt.grid(True)
plt.show()
```

### Latent Space Visualization

After training the model, the latent space can be visualized using **PCA** to reduce the dimensionality of the latent vectors to 2D.

```python
from sklearn.decomposition import PCA

latent_vectors = np.vstack(latent_vecs)  # Latent vectors collected from the test set
pca = PCA(n_components=2)
latent_2d = pca.fit_transform(latent_vectors)

plt.scatter(latent_2d[:, 0], latent_2d[:, 1], s=20, alpha=0.8)
plt.title('PCA of Latent Space')
plt.xlabel('PC1')
plt.ylabel('PC2')
plt.grid(True)
plt.show()
```

---

## Usage

### 1. Download the dataset:

```bash
kaggle datasets download zaharch/quantum-machine-9-aka-qm9
```

### 2. Run the notebook:

1. Clone or download this repository.
2. Open the `Molecule.ipynb` notebook in **Jupyter**.
3. Follow the steps in the notebook to:

   * Download the dataset.
   * Preprocess the data.
   * Train the VAE model.
   * Generate new molecules.
   * Visualize the results.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Contact

If you have any questions or issues with the code, feel free to open an issue or reach out to me.

---

### Example `requirements.txt`

```
torch
torchvision
torch-geometric
rdkit
tqdm
pandas
numpy
matplotlib
seaborn
```
