# 🧠 SCC0270 - Neural Networks

Repository containing projects for the course **SCC0270 - Neural Networks and Deep Learning**, offered by ICMC - USP São Carlos, 2025.

## 🔧 Environment Setup

This project uses the [`uv`](https://github.com/astral-sh/uv) environment and dependency manager.  
To set up the environment correctly, follow the steps below:

### 1. Install `uv`

If you don't have `uv` installed yet, run:

```bash
curl -Ls https://astral.sh/uv/install.sh | bash
```

### 2. Create and sync the virtual environment

To create a virtual environment and install all dependencies listed in `pyproject.toml`, run:

```bash
uv sync
```

This command automatically creates the virtual environment and installs the necessary libraries.

---

## 📂 Repository Organization

- `data/` — 📦 Data used in projects  
- `notebooks/` — 📒 Development and analysis notebooks  
- `model/` - 🔧 ResNet50 models
- `posters/` — 🖼️ Graphic materials (posters and presentations)  
- `assets/` — 📁 Auxiliary files such as images used in README  

---

# Projects

The projects are based on the following paper: ["MedPix 2.0: A Comprehensive Multimodal Biomedical Dataset for Advanced AI Applications"](https://arxiv.org/html/2407.02994v1#S3)

The authors' goal was to build a better-structured dataset with medical images and clinical information associated with each image in order to provide a data source so that researchers and healthcare professionals can build AI solutions for the medical field, given that a standardized dataset is available.

The authors' main motivation stems from the fact that most medical data is private and, until the publication of the work, there were no open medical datasets whose structure allowed for the development of AI-based solutions.

## Data (MedPix 2.0)

This project uses the **MedPix 2.0** dataset, a comprehensive, high-quality multimodal biomedical database developed for advanced Artificial Intelligence (AI) applications in the medical domain. Originating from the well-known MedPix® database (used for Continuing Medical Education), MedPix 2.0 was created to overcome the scarcity of high-quality, publicly accessible medical datasets, especially for the development of Multimodal Large Language Models (MLLM).

The construction of MedPix 2.0 involved a **semi-automatic pipeline for extracting visual and textual data**, followed by a **manual curation process** to remove noisy samples. The data is stored in a **non-relational MongoDB database**, which reorganizes the original MedPix® structure, making it more accessible and structured for AI applications.

### Data Structure

MedPix 2.0 integrates **visual data (Computed Tomography - CT and Magnetic Resonance - MR scans)** and **textual data (clinical reports and findings)**. Each clinical case within the dataset contains at least one medical image and corresponding information, such as findings, discussion notes, diagnosis, differential diagnosis, treatment and follow-up, all presented in a semi-structured JSON format.

In the MongoDB implementation, the data is organized into two main collections:

- **`Image_Descriptions`**: Contains **description documents**, which store information strictly connected to the images. These documents include details such as the examination modality (CT or MR) and body part (Location).
- **`Clinical_reports`**: Contains **case-topic documents**, which group detailed information from a complete clinical case, including academic and general explanations about the investigated disease.

There is a one-to-many relationship between clinical cases and images, where the unique identifier (`U_id`) of a case-topic document is embedded in each related image description document. The images themselves are stored in the `MedPix-2.0/images/` folder and accessed via URL.

### Data Split and Access in the Project

For this project, a subset of MedPix 2.0 data was divided into **training and test sets** and is present in the `MedPix-2.0/splitted_dataset/` folder. The data is provided in JSON files, which follow the structure of the documents described above. Examples of these files include:

- **`descriptions_train.jsonl`**: Description documents for the training set.
- **`data_train.jsonl`**: Case-topic documents for the training set.
- **`descriptions_test.jsonl`**: Description documents for the test set.
- **`data_test.jsonl`**: Case-topic documents for the test set.

This data structuring facilitates direct use for training and fine-tuning Machine Learning and Deep Learning models, without the need for additional preprocessing for multimodal tasks. The MedPix 2.0 database, with its curation and structuring, is a relevant starting point for developing multimodal AI systems in the medical domain, including information extraction systems, automated image analysis, and generative AI models for clinical reports.

The project source code and the data used for testing and training are **freely available** in the public repositories indicated in the original article.

## 🚀 Project 01

📅 **Deadline:** June 21, 2025  

**Tasks:**  

- 🔍 Binary classifier for **Modality** (`CT` or `MR`)  
- 🧠 Multi-class classifier for **Location** (21 classes)  

The objective of Project 01 is to build a binary classifier capable of classifying images as `CT` (Computed Tomography) or `MR` (Magnetic Resonance) and also a multi-class classifier to classify the Locations associated with each image. There are 21 available Locations: `Chest, Pulmonary`, `Genitourinary`, `Head and Neck`, `Cardiovascular`, `Brain and Neuro`, `Abdomen`, `Spine`, `Eye and Orbit`, `Gastrointestinal`, `Vascular`, `Endocrine`, `Musculoskeletal`, `Pathology`, `Generalized`, `Hematopoietic`, `Dental, Oral, or Tooth`, `Nerve, central`, `Breast and Mammography`, `Bethesda, MD`, `Ophthalmology`, `Nerve, peripheral`.

Initially, in the `Trabalho_01_ML.ipynb` notebook, classic ML models were built based on feature extraction techniques such as Texture and Color Descriptors, followed by KNN fitting.
In the notebook available on Colab at [CNN Training Notebook for Modality on Colab](https://drive.google.com/drive/folders/1nnpJwP1hIiqQjFYWDabOCFGvpeqj7dPg?usp=drive_link), fine-tuning of a ResNet50 for modality classification was performed, and in the notebook [CNN Training Notebook for Location on Colab](https://colab.research.google.com/drive/1X9ANeqFUI9rEleWq8OYalE2q5EnB3cQ_?usp=drive_link), fine-tuning of a ResNet50 for location classification was performed.

The code for loading the trained model and calculating evaluation metrics is present in the `Trabalho_01_CNN.ipynb` notebook.

We obtained the following results in the binary classification task:

## Modality Classification Results

### KNN + Texture Descriptors

| Class        | Precision | Recall | F1-Score | Support |
|--------------|-----------|--------|----------|---------|
| MR           | 0.00      | 0.00   | 0.00     | 100     |
| CT           | 0.50      | 1.00   | 0.67     | 100     |
| **Accuracy** |           |        | 0.50     | 200     |
| **Macro Avg**| 0.25      | 0.50   | 0.33     | 200     |
| **Weighted Avg** | 0.25  | 0.50   | 0.33     | 200     |

### KNN + Image Descriptors

| Class          | Precision | Recall | F1-Score | Support |
|----------------|-----------|--------|----------|---------|
| MR             | 0.60      | 0.64   | 0.62     | 100     |
| CT             | 0.62      | 0.58   | 0.60     | 100     |
| **Accuracy**   |           |        | 0.61     | 200     |
| **Macro Avg**  | 0.61      | 0.61   | 0.61     | 200     |
| **Weighted Avg**| 0.61     | 0.61   | 0.61     | 200     |

### KNN + Texture + Image Descriptors

| Class           | Precision | Recall | F1-Score | Support |
|-----------------|-----------|--------|----------|---------|
| MR               | 0.00      | 0.00   | 0.00     | 100     |
| CT               | 0.50      | 1.00   | 0.67     | 100     |
| **Accuracy**     |           |        | 0.50     | 200     |
| **Macro Avg**    | 0.25      | 0.50   | 0.33     | 200     |
| **Weighted Avg** | 0.25      | 0.50   | 0.33     | 200     |

### Fine-tuning ResNet50 (02 epochs)

| Class           | Precision | Recall | F1-Score | Support |
|-----------------|-----------|--------|----------|---------|
| MR               | 0.99      | 0.98   | 0.98     | 100     |
| CT               | 0.98      | 0.99   | 0.99     | 100     |
| **Accuracy**     |           |        | 0.98     | 200     |
| **Macro Avg**    | 0.99      | 0.98   | 0.98     | 200     |
| **Weighted Avg** | 0.99      | 0.98   | 0.98     | 200     |

## Location Classification Results (ResNet50 - 02 Epochs)

ML methods with texture and color descriptors were not able to classify the Location of the images.
Similarly, a ResNet50 was trained for the classification of 21 locations.

| Label                  | Precision | Recall | F1-Score | Support |
|------------------------|-----------|--------|----------|---------|
| Chest, Pulmonary       | 0.00      | 0.00   | 0.00     | 1       |
| Genitourinary          | 0.68      | 0.94   | 0.79     | 51      |
| Head and Neck          | 0.00      | 0.00   | 0.00     | 4       |
| Cardiovascular         | 0.00      | 0.00   | 0.00     | 1       |
| Brain and Neuro        | 0.70      | 0.88   | 0.78     | 24      |
| Eye and Orbit          | 1.00      | 0.29   | 0.44     | 7       |
| Gastrointestinal       | 0.75      | 0.29   | 0.42     | 31      |
| Vascular               | 0.00      | 0.00   | 0.00     | 10      |
| Pathology              | 0.18      | 0.27   | 0.21     | 11      |
| Spine                  | 0.43      | 0.30   | 0.35     | 10      |
| Endocrine              | 0.48      | 0.71   | 0.57     | 14      |
| Nerve, central         | 0.00      | 0.00   | 0.00     | 4       |
| Musculoskeletal        | 0.82      | 0.69   | 0.75     | 26      |
| Abdomen                | 0.00      | 0.00   | 0.00     | 6       |
| **Accuracy**           |           |        | **0.57** | 200     |
| **Macro Avg**          | 0.36      | 0.31   | 0.31     | 200     |
| **Weighted Avg**       | 0.58      | 0.57   | 0.54     | 200     |

We obtained an **accuracy of 57%** which is slightly higher than the 52.5% reported by the authors.  
Since not all classes are present in the test set, the table above does not show all classes.
It can be observed that Locations with fewer examples available in the training set show lower accuracy values, which is expected.
We did not observe significant improvements with increasing the number of epochs beyond 2.

---

## 🚧 Project 02

🗓️ **Deadline:** July 09, 2025\
🔄 **Multimodal Classifier with CLIP**

The objective of Project 02 was to develop an **Anatomical Location** classifier using a **multimodal** approach, combining visual information (medical image) and textual information (associated caption) through OpenAI's **CLIP (Contrastive Language-Image Pretraining)** model.

Initially, a custom dataset was created from the MedPix 2.0 JSON files, containing the association between image, caption (`Caption`) and anatomical location (`Location`). The data was processed and divided into training and test sets, with images being loaded and processed along with texts using the `CLIPProcessor`. The training code can be accessed in the following Colab notebook [CLIP Training Notebook for Multimodality](https://colab.research.google.com/drive/1LqGHXRNEwuTu8tcZP0jmpvhhN7siQJ8O?usp=sharing).

The model was built in two stages:

- Extraction of image and text **embeddings** via the pre-trained model `clip-vit-base-patch32`;
- Training a **simple classifier** with linear layers on top of the concatenated embeddings.

---

### 📈 Training Curves

During the 30 epochs, a **consistent decrease in training and validation losses** was observed, without signs of overfitting.

| Epoch | Training Loss | Validation Loss |
| ----- | ------------- | --------------- |
| 1     | 2.7800        | 2.4204          |
| 10    | 1.3309        | 1.3012          |
| 20    | 1.0183        | 1.0025          |
| 30    | 0.8183        | 0.8064          |

> 🔍 The difference between the curves was small throughout training, indicating good generalization. Even after 30 epochs, both still show a downward trend.

---

### 📋 Results by Class (classification\_report)

The model achieved a **total accuracy of 78%**, surpassing the baseline of 52.5% reported in the original article. Below are the main results by class:

| Class                    | Precision | Recall | F1-Score | Support |
|---------------------------|-----------|--------|----------|---------|
| Brain and Neuro           | 0.80      | 0.97   | 0.88     | 522     |
| Musculoskeletal           | 0.85      | 0.95   | 0.89     | 209     |
| Chest, Pulmonary          | 0.79      | 0.95   | 0.86     | 187     |
| Gastrointestinal          | 0.65      | 0.95   | 0.77     | 154     |
| Genitourinary             | 0.71      | 0.83   | 0.77     | 120     |
| Cardiovascular            | 0.89      | 0.66   | 0.76     | 50      |
| Spine                     | 0.78      | 0.65   | 0.70     | 48      |
| Eye and Orbit             | 0.88      | 0.57   | 0.69     | 49      |
| Head and Neck             | 0.77      | 0.58   | 0.66     | 76      |
| Generalized               | 1.00      | 0.11   | 0.19     | 56      |
| Abdomen                   | 1.00      | 0.10   | 0.17     | 42      |
| Vascular                  | 0.82      | 0.27   | 0.41     | 66      |
| Classes with support < 10 | 0.00      | 0.00   | 0.00     | -       |
| **Total Accuracy**        |           |        | **0.78** | 1653    |
| **Macro Average**         | 0.47      | 0.36   | 0.37     |         |
| **Weighted Average**      | 0.76      | 0.78   | 0.74     |         |

> 📌 **Analysis:**
>
> - The model performed very well on classes with higher data volume (e.g., `Brain and Neuro`, `Musculoskeletal`).
> - Classes with few examples had very low or zero performance, as expected.
> - The **weighted average** shows that, even with imbalance, the model was effective in the overall multimodal classification task.

---

## 🎯 Project 03

📅 **Deadline:** June 11, 2025  

Extension Activity on the USP campus - São Carlos (ICMC) with poster presentation:  
🖼️ **"How do machines see?"**  

**File:** `posters/Poster Rede Neurais - Final.pptx`  

**Poster Image**  
![poster-image](assets/imagem_poster.png)  

---

## 👥 Team Members

- Brunna Quatrochi [🔗 LinkedIn](https://www.linkedin.com/in/brunna-quatrochi/)
- Gabriela dos Santos Amaral 🐙 [GitHub](https://github.com/GabrielaSAmaral) | [🔗 LinkedIn](https://www.linkedin.com/in/gabriela-amaral-ga/)
- Heitor Carvalho Pinheiro 🐙 [GitHub](https://github.com/Heitorcp) | [🔗 LinkedIn](https://www.linkedin.com/in/heitor-cp/)
- Ivan Barbosa Pinto 🐙 [GitHub](https://github.com/ivpinheiro) | [🔗 LinkedIn](https://www.linkedin.com/in/ivanpinheiro/)
- João Pedro Serpellone
- Leo Gianotti [🔗 LinkedIn](https://www.linkedin.com/in/leo-gianotti-48124a20a/)
- Matheus Chaves Silva [🔗 LinkedIn](https://www.linkedin.com/in/matheus-chaves-silva-86425913a/)
- Murilo Valentim Zabott
