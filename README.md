# Scalable Medical Image Preprocessing using Apache Beam & Google Cloud

![WhatsApp Image 2025-05-18 at 18 42 43](https://github.com/user-attachments/assets/8975a2b9-8092-4491-9ec7-6a6d91a55e52)
![Medical Image Processing Dataflow x Apache Beam](https://github.com/user-attachments/assets/c2f3e70b-7b22-4138-bd77-2cfcd59d5d1f)

## Overview

This project demonstrates a scalable preprocessing pipeline for Whole Slide Images (WSIs) used in medical diagnosis. WSIs are gigapixel images (up to **11 GB per file**) and pose a significant challenge for conventional image processing tools. We use **Apache Beam** and **Google Cloud Dataflow** to handle the distributed processing and prepare the data in a format ready for ML model training on **Vertex AI**.

---

## What We're Solving

- **Large WSI images** that exceed in-memory processing limits  
- **Multiple images per patient** with a **single label** (Multiple Instance Learning)  
- **Background and noise** in images need to be removed to extract meaningful tiles  
- **Efficient data serialization** into `TFRecord` format for downstream ML tasks  
- **Scalable preprocessing pipeline** capable of handling thousands of such images in parallel  

---

## Dataset

We used the Mayo Clinic Dataset from Kaggle. It consists of:

- **CSV Metadata**:  
  Contains `image_id`, `center_id`, `patient_id`, `image_num`, and `label`.  
  > Note: Multiple rows (images) per patient. Label is **assigned at patient level**.

- **TIFF Images**:  
  Gigapixel whole slide images of tissue slices.  
  > Background regions are present and need to be filtered out.

---

## Architecture

### Pipeline Steps

1. **Read & Parse CSV** to group image rows by patient  
2. **Load TIFF images** corresponding to each row  
3. **Extract Tiles** from relevant regions  
4. **Merge Tiles + Patient Info**  
5. **Serialize into `tf.train.Example`**  
6. **Write to TFRecord** format in Google Cloud Storage  

> Executed on **Apache Beam**, running via **Google Cloud Dataflow**

---

## Tech Stack

| Component | Description |
|----------|-------------|
| **Apache Beam** | Unified programming model for large-scale data processing |
| **Google Cloud Dataflow** | Serverless runner for Beam pipelines |
| **Google Cloud Storage** | Input TIFFs and output TFRecord storage |
| **TensorFlow** | Used for `TFRecord` serialization |
| **OpenSlide** | To handle reading of `.tif` Whole Slide Images |
| **Kaggle Datasets** | Source of Mayo Clinic pathology image data |

---

## References

- [Kaggle Mayo Clinic Challenge](https://www.kaggle.com/competitions/mayo-clinic-strip-ai)
- [Big Medical Image Preprocessing with Apache Beam](https://dlabs.ai/blog/big-medical-image-preprocessing-with-apache-beam/)
