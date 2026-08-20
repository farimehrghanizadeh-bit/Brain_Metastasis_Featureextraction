# Brain_Metastasis_Featureextraction
# Radiomic Feature Extraction — Brain Metastasis MRI

Extracts quantitative radiomic features from segmented brain metastasis
tumors, covering all four feature categories specified in the project
protocol: **shape, location, intensity, and texture**. Output feeds
into a planned downstream classification of primary tumor origin.

## Results
- 107 shape, intensity, and texture features (PyRadiomics defaults) +
  3 location features (tumor centroid in shared template space)
- 43/43 patients successfully processed
- Output saved as a single CSV, one row per patient

## Method
Adapted from the official PyRadiomics example notebook
([AIM-Harvard/pyradiomics](https://github.com/AIM-Harvard/pyradiomics/blob/master/notebooks/RadiomicsExample.ipynb),
BSD-3-Clause License), modified for:
- Full-cohort batch processing (43 patients), not a 2-image demo
- Binary mask handling (source masks use a clinical multi-lesion
  labeling scheme, not simple 0/1 labels)
- An added location feature category (tumor centroid), since
  PyRadiomics itself only computes shape/intensity/texture
- CSV output for downstream statistical/ML analysis

## Protocol feature coverage

| Category | Covered | Source |
|---|---|---|
| Shape | Yes | PyRadiomics `original_shape_*` |
| Intensity | Yes | PyRadiomics `original_firstorder_*` |
| Texture | Yes | PyRadiomics GLCM / GLSZM / GLRLM / GLDM / NGTDM |
| Location | Yes | Added — tumor centroid in shared MNI template space |

Edema-based features (from FLAIR/T2, per the protocol) are not yet
available, since the current pipeline uses a single modality
(T1 contrast-enhanced). Planned as part of the multi-modality
extension described in the [segmentation repo](https://github.com/farimehrghanizadeh-bit/Brain_Metastasis_Segmentation).

## Data
Sourced from the MolAB Brain Metastasis Dataset
(https://molab.es/datasets-brain-metastasis-1/?type=metasrd). Citation:
Ocaña-Tienda, B., et al. A comprehensive dataset of annotated brain
metastasis MR images with clinical and radiomic data. Scientific Data
10, 208 (2023). https://doi.org/10.1038/s41597-023-02123-0

Features are extracted from the radiologist ground-truth tumor masks
(not the segmentation model's predictions), for maximum boundary
accuracy in the downstream analysis.

## Environment
Run in an isolated Python environment (`radiomics_env`, Python 3.10)
separate from the preprocessing/segmentation environment, since
PyRadiomics required an older Python version and a from-source
compiled C extension.

## Requirements
pyradiomics==3.0.1
SimpleITK
numpy
pandas
matplotlib
