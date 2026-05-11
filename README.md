<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/030b6822-0d94-427b-a81d-f20e8729741f" />

# Spectral-Token Transformer Ground Motion Model (STT-GMM)

This repository contains the implementation, training, evaluation, and executable workflow for the **Spectral-Token Transformer Ground Motion Model (STT-GMM)** developed for response spectrum prediction using the NGA-West2 database.  
The model represents the response spectrum as a structured sequence and learns cross-period dependencies using an attention-based architecture, while remaining consistent with classical ground motion modeling principles.

---

## Repository Structure

The repository consists of the following key files:

### 1. `input_normalization_stats.csv`
This file contains the **training-time normalization statistics** for the continuous input parameters:
- Moment magnitude (Mw)
- Hypocentral depth
- Joyner–Boore distance (RJB)
- log(RJB)
- log(Vs30)

**Purpose**
- Ensures consistency between training and inference.
- Must be loaded during execution to correctly normalize input features before prediction.

> ⚠️ **Important:**  
> Failure to apply these normalization statistics during inference will result in physically inconsistent predictions (e.g., loss of magnitude scaling, distance attenuation, or site effects).

---

### 2. `stt_gmm_weights.pt`
This file stores the **trained model bundle**, including:
- Spectral-token transformer weights
- Context encoder weights
- Event embedding weights
- Model configuration (architecture hyperparameters)
- Output normalization statistics (mean and standard deviation of log spectral accelerations)
- Spectral periods
- Event ID mapping used during training

**Purpose**
- Enables reproducible inference without retraining.
- Designed to be loaded directly by the executable notebook.

---

### 3. `llm_exec.ipynb`
This notebook provides an **end-to-end executable pipeline** for model inference.

**Key functionalities**
- Loads:
  - `input_normalization_stats.csv`
  - `stt_gmm_weights.pt`
- Reconstructs the trained model architecture
- Accepts user-defined seismic input parameters:
  - Mw, depth, RJB, Vs30, fault type, motion direction, event ID
- Produces:
  - Predicted log response spectrum
  - Physical response spectrum plots (SA vs period)

**Intended use**
- Apply the trained STT-GMM to:
  - Known events (event-conditioned prediction)
  - New events (median prediction)
- Demonstrate physically consistent trends in spectral response

This notebook is the **primary entry point** for users who wish to use the trained model without modifying the training code.

---

### 4. `model_training.ipynb`
This notebook contains the **complete training pipeline** for the Spectral-Token Transformer model.

**Contents**
- Data loading and preprocessing
- Feature normalization
- Spectral token construction
- Model architecture definition
- Training loop with:
  - Loss computation
  - Gradient backpropagation
  - Optimization
  - Early stopping
- Model checkpointing and saving

**Intended use**
- Reproduce the results reported in the accompanying paper
- Modify architecture or hyperparameters
- Extend the framework to other datasets or tectonic regions

---

### 5. `prelim_analysis.ipynb`
This notebook provides **quick diagnostic and validation tools** for model evaluation.

**Includes**
- R² scores for spectral acceleration at individual periods
- k-fold cross-validation results
- Basic performance plots

**Purpose**
- Allow rapid verification of model performance after training
- Serve as a sanity-check tool for users experimenting with the training code
- Help identify overfitting or data leakage issues early in development

---

## Typical Workflow

1. **Train the model (optional)**
   - Use `model_training.ipynb` to train the STT-GMM from scratch.

2. **Quick validation**
   - Run `prelim_analysis.ipynb` to inspect R² scores and cross-validation performance.

3. **Inference / application**
   - Use `llm_exec.ipynb` to:
     - Load the trained model
     - Normalize inputs using `input_normalization_stats.csv`
     - Generate predicted response spectra

---

## Notes and Best Practices

- Always use the provided normalization statistics for inference.
- Event-conditioned predictions are valid **only for events seen during training**.
- For new or unseen earthquakes, the model automatically produces **median ground motion predictions**, consistent with classical GMM usage.
- The framework is designed for **engineering and seismic hazard applications**, not record-level waveform reconstruction.

---

## Citation

If you use this repository in academic work, please cite the accompanying paper describing the Spectral-Token Transformer Ground Motion Model.

---

## Contact

For questions, extensions, or collaboration inquiries, please refer to the corresponding author information provided in the associated publication.


*Created in May 2026*

*@author: Pavan Mohan Neelamraju*

*Affiliation: Indian Institute of Technology Madras*

**Email**: npavanmohan3@gmail.com

**Personal Website 🔴🔵**: [pavanmohan.netlify.app](https://pavanmohan.netlify.app/)
