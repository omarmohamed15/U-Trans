<p align="center">
  <img src="assets/logo.png" width="800"/>
</p>

# U-Trans  
## A Foundation Model for Seismic Waveform Representation

U-Trans is a **foundation model for seismic waveform representation** designed to learn transferable, structured embeddings from raw 3-component seismic waveforms.

This repository provides:

- ✅ U-Trans foundation backbone  
- ✅ Self-supervised foundation training pipeline  
- ✅ Latent feature extraction  
- ✅ Modular downstream task architectures  
- ✅ Unified seismic dataset construction pipeline  

---

# 🌍 Overview

U-Trans follows a **foundation model paradigm**:

1. Learn a strong, general seismic representation.
2. Expose reusable latent features.
3. Attach modular downstream models for task-specific learning.

The architecture separates:

```
Foundation representation  →  Latent features  →  Downstream models
```

---


# 🛠 Environment Setup

To ensure full reproducibility, this repository provides a YAML configuration file containing the exact conda environment used for development and training.

The environment file includes:

- Python version  
- TensorFlow and deep learning dependencies  
- Scientific computing libraries  
- Visualization tools  
- Required utility packages  

## 📦 Create the Environment

After cloning the repository, create the environment using:

```bash
conda env create -f Foundation.yml
```

Then activate it:

```bash
conda activate Foundation
```

This guarantees compatibility with:

- Foundation training
- Downstream tasks
- Data processing
- Evaluation pipelines

Using the provided YAML file ensures consistent results across different systems and hardware setups.

---

# 📂 Data Preparation

A dedicated `data/` folder is provided to construct a unified seismic dataset for foundation training and downstream experiments.

The combined dataset is built from:

- **STEAD** – https://github.com/smousavi05/STEAD  
- **INSTANCE** – https://github.com/INGV/instance  
- **TXED** – https://github.com/chenyk1990/txed  

## 🔽 Download Instructions

1. Download all three datasets from their official repositories.
2. Create a folder in your project directory called:

```
Data_Seismic
```

3. Place the downloaded datasets inside:

```
Data_Seismic/
    ├── STEAD/
    ├── INSTANCE/
    └── TXED/
```

4. Run the notebook inside the `data/` folder to merge them.

This generates a unified HDF5 file:

```
DataCollected
```

All waveforms are standardized to:

```
(6000, 3)
```

The combined dataset is used for:

- Foundation pretraining  
- P-wave picking  
- S-wave picking  
- Magnitude estimation  
- Event location  
- Polarity classification  

---

# 🧠 Architecture

## 🔹 U-Trans Foundation Backbone

The foundation model consists of:

- U-Net encoder–decoder architecture  
- Transformer bottleneck representation  
- Multi-scale waveform feature extraction  
- Learned latent token embeddings  

---

### 📥 Input

```
(B, 6000, 3)
```

Where:

- `B` = batch size  
- `6000` = waveform length  
- `3` = three-component seismic signal  

---

### 🔹 Latent Representation

U-Trans produces:

```
(B, 75, 80)
```

- 75 latent tokens  
- 80-dimensional embedding per token  

These tokens encode structured waveform representations using transformer layers.

---

### 🔹 Decoder Feature Stream

The foundation model can also output:

```
(B, 6000, 1)
```

This stream can be directly concatenated with downstream task models.

---
# 🚀 Using the Foundation Model

Load pretrained weights and extract features:

```python
import os
import sys
import numpy as np

sys.path.insert(0, os.path.abspath(".."))

from utrans.foundation import get_latent_model, get_decoder_model

UNET_WEIGHTS = "../weights/UTrans_Foundation.h5"

latent_model = get_latent_model(UNET_WEIGHTS)

ready_to_concatenate_model, Featuear_Ready_to_Concatenate = \
    get_decoder_model(UNET_WEIGHTS)
```

### Outputs

- Latent tokens → `(B, 75, 80)`  
- Decoder features → `(B, 6000, 1)`  

---

# 🔬 Design Philosophy

U-Trans is designed to:

- Learn general waveform representations  
- Enable modular downstream experimentation  
- Separate representation learning from task-specific modeling  
- Support transformer-based extensions  
- Scale to multiple seismic tasks  

---


# 🏗 Repository Structure

```
.
├── assets/
│   └── logo.png
│
├── data/
│   ├── Collected_Large_Dataset.ipynb
│   └── README.md
│
├── train/
│   ├── U-Trans_Train.ipynb
│   ├── EqT_utils_Recon.py
│   ├── IDS_Collected_Data_train.npy
│   ├── IDS_Collected_Data_valid.npy
│   └── IDS_Collected_Data_test.npy
│
├── examples/
│   ├── foundation_usage.ipynb
│   └── downstream/
│       ├── pwave_eqcct/
│       ├── swave_eqcct/
│       ├── magnitude_ViT/
│       ├── location_ConvMixer/
│       └── polarity_CCT/
│
├── utrans/
│   ├── foundation.py
│   ├── preprocessing.py
│   └── layers.py
│
├── weights/
│   └── UTrans_Foundation.h5
│
└── README.md
```

---

# 🧪 Foundation Training

The `train/` folder contains the **self-supervised reconstruction training pipeline** used to train U-Trans.

During training, waveforms are intentionally corrupted in:

- Time domain  
- Frequency domain  

The model learns to reconstruct the original waveform, enabling robust and transferable representation learning.

## 📂 Train Folder Structure

```
train/
├── U-Trans_Train.ipynb
├── EqT_utils_Recon.py
├── IDS_Collected_Data_train.npy
├── IDS_Collected_Data_valid.npy
└── IDS_Collected_Data_test.npy
```

### File Descriptions

- `U-Trans_Train.ipynb`  
  Main notebook for training the U-Trans foundation model.

- `EqT_utils_Recon.py`  
  Utilities for:
  - Time-domain corruption  
  - Frequency-domain corruption  
  - Data generators  
  - Training configuration  

---

# 🔌 Downstream Architectures

Example downstream models are provided inside:

```
examples/downstream/
```

Available architectures include:

- `pwave_eqcct/` → Transformer-based P-wave picking (EQCCT)  
- `swave_eqcct/` → Transformer-based S-wave picking (EQCCT)  
- `magnitude_ViT/` → Magnitude estimation (ViT)  
- `location_ConvMixer/` → Event location (ConvMixer)  
- `polarity_CCT/` → Polarity classification (CCT)  

Each downstream module attaches to the U-Trans latent or decoder representation.

---

# 📌 Key Characteristics

- U-Net multi-scale encoder  
- Transformer bottleneck modeling  
- Token-based latent embedding  
- Modular downstream integration  

---

## 📎 Citation

If you use this repository in your research, please cite the following paper:

Saad, O. M., Chen, Y., & Alkhalifah, T. (2026).  
**U-Trans: a foundation model for seismic waveform representation and enhanced downstream earthquake tasks.**  
*Scientific Reports*.  
https://doi.org/10.1038/s41598-026-41454-x

### BibTeX

```bibtex
@article{saad2026u,
  title={U-Trans: a foundation model for seismic waveform representation and enhanced downstream earthquake tasks},
  author={Saad, Omar M and Chen, Yangkang and Alkhalifah, Tariq},
  journal={Scientific Reports},
  year={2026},
  publisher={Nature Publishing Group UK London},
  doi={10.1038/s41598-026-41454-x}
}




