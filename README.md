# SpeechSynthesisProject

Here’s a comprehensive `README.md` file to include with your project, detailing the setup, requirements, and instructions for training the model in Google Colab.

---

# Speech Synthesis Model with VITS

This project is a non-Hindi language speech synthesis model trained using the [VITS architecture](https://github.com/jaywalnut310/vits) on data from the AI4Bharat dataset. This guide provides step-by-step instructions on setting up the environment, preparing data, and training the model.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Data Preparation](#data-preparation)
- [Training the Model](#training-the-model)
- [Evaluation and Inference](#evaluation-and-inference)
- [Troubleshooting](#troubleshooting)
- [Credits](#credits)

---

## Prerequisites

- **Google Colab** (recommended) with GPU enabled or a local machine with a compatible NVIDIA GPU.
- **GitHub** for cloning the VITS repository.
- **Google Drive** (optional) for saving data and model checkpoints persistently.

## Environment Setup

### 1. Enable GPU in Google Colab

In Google Colab:
1. Go to **Runtime > Change runtime type**.
2. Select **GPU** in the Hardware accelerator dropdown.
3. Click **Save**.

### 2. Clone the VITS Repository

Open a new Colab cell and run:

```bash
!git clone https://github.com/jaywalnut310/vits.git
%cd vits
```

### 3. Install Dependencies

Some dependencies in the `requirements.txt` file may be outdated or incompatible with Colab. Install dependencies manually as follows:

```python
!pip install Cython librosa matplotlib numpy scipy soundfile unidecode phonemizer
!pip install torch==2.0.1+cu118 -f https://download.pytorch.org/whl/torch_stable.html
```

### 4. Compile `monotonic_align` Module

The `monotonic_align` module needs to be compiled for alignment calculations.

```bash
%cd /content/vits/monotonic_align
!python setup.py build_ext --inplace
%cd /content/vits
```

## Data Preparation

1. **Download and Organize AI4Bharat Dataset**: 
   - Download the dataset from AI4Bharat. Ensure that your dataset includes `audio` files and metadata in `.csv` or `.json` format.

2. **Filter Data for Target Language**:
   - Use the `meta_speaker_stats.csv` and filter rows to select the target language, e.g., `"Dogri"`.
   
3. **Script for Filtering Data**:
   - Save a script like `filter_data.py` to filter the dataset and create a manifest file. This script will:
     - Copy relevant audio files.
     - Update the manifest file to include only files in the target language.

## Training the Model

### 1. Configure `configs/ljs_base.json`

Edit the configuration file `configs/ljs_base.json` to match your dataset's specifics, like `sampling_rate`, `training_files`, and `validation_files`. 

Key configurations:
- `training_files`: Path to the training manifest file.
- `validation_files`: Path to the validation manifest file.
- `sampling_rate`: Sampling rate of the audio files in your dataset.
  
### 2. Start Training

Run the following command to start training:

```bash
!python train.py -c configs/ljs_base.json -m my_model
```

Checkpoints will be saved in the `my_model` directory. You can monitor training logs for loss values and other metrics.

## Evaluation and Inference

Once the model is trained, you can perform inference to generate speech from text.

1. **Run Inference Script** (ensure paths are updated for checkpoints):

```python
!python inference.py -c configs/ljs_base.json -m my_model -t "Your text here"
```

The generated audio files will be saved in the output directory specified in the script.

## Troubleshooting

- **GPU Not Detected**: Ensure GPU is enabled in Colab and verify with `torch.cuda.is_available()`.
- **Dependency Errors**: Update `requirements.txt` or manually install dependencies as outlined above.
- **Path Issues**: Ensure all file paths (for data, manifest, and checkpoints) are correctly specified in configuration files.

## Credits

- VITS Model by [Jaywalnut](https://github.com/jaywalnut310/vits)
- AI4Bharat Dataset

---

### Notes:
- This README assumes that you are using Google Colab, so adjust paths if using a different environment.
- Always ensure compliance with dataset usage policies for AI4Bharat and any other datasets you use.

This guide provides a step-by-step approach to training a speech synthesis model with VITS on a custom dataset. Follow it closely to avoid common setup issues!
