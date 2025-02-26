# Patch Extractor

## Overview

`patch_extractor.py` is a Python script designed to extract 2D patches from 3D medical imaging data stored in `.nii.gz` format. The extracted patches can be used for training machine learning models, performing image analysis, or other research applications.

The script allows users to specify parameters such as patch size, label percentage constraints, and targeted planes (axial, sagittal, coronal) while ensuring extracted patches contain meaningful information based on provided label criteria.


## Features

- Extracts 2D patches from 3D medical images (`.nii.gz` format).

- Supports multiple planes: axial (z), sagittal (x), and coronal (y).

- Filters patches based on label percentage and presence of specific labels.

- Saves extracted patches as `.jpg` images for easy visualization.

- Generates JSON metadata for each patch, including label statistics.

- Supports optional 4x4 subdivision of patches.

- Ensures compatibility with NIfTI formats using `nibabel`.

## Requirements

To run `patch_extractor.py`, you need the following dependencies:
```python
pip install nibabel numpy matplotlib
```

## Imported Packages

The script uses the following Python libraries:
```python
import nibabel as nib
import numpy as np
import os
import random
import json
from datetime import datetime
import matplotlib.pyplot as plt
```
## Usage

### Extracting Patches

Run the script using the following example:

from patch_extractor import patch_extractor
```python
patch_extractor(
    data_file="data/data_238.nii.gz",
    label_file="data/label_238.nii.gz",
    output_dir="output_patches",
    num_patches=9,
    patch_size_range=(100, 300),
    label_percentage_range=(5, 20),
    planes=('z',),
    layer_range={'z': (50, 400)}, # Total slices - X: 640, Y: 640, Z: 419
    labels_present=[1, 4, (8, 15)]
)
```


### Extracting 4x4 Subdivided Patches

```python
from patch_extractor import patch_extractor_4x4

patch_extractor_4x4(
    data_file="data/data_238.nii.gz",
    label_file="data/label_238.nii.gz",
    output_dir="output_patches_4x4",
    num_patches=5,
    patch_size_range=(400, 400),
    label_percentage_range=(7, 22),
    plane="z",
    layer_range=(10, 400),
    labels_present=(1, 3, 6, 8)
)
```

## Parameters

- `data_file`: Path to the `.nii.gz` file containing the medical imaging data.

- `label_file`: Path to the corresponding label `.nii.gz` file.

- `output_dir`: Directory where extracted patches and metadata will be stored.

- `num_patches`: Number of patches to extract.

- `patch_size_range`: Tuple specifying the minimum and maximum patch size.

- `label_percentage_range`: Range of valid label percentages to filter patches.

- `plane`: Tuple specifying which planes (`z`, `x`, `y`) to extract patches from.

- `slice_range`: Dictionary specifying layer ranges for each plane.

- `labels_present`: List of required labels in the patch (supports ranges using tuples).

- `generate_all_planes`: If `True`, extracts patches from all planes regardless of `plane` parameter.

## Output

Each extracted patch will be stored in the output_dir with the following:

`.jpg` images for easy visualization.

`.nii.gz` format for compatibility with medical imaging tools.

JSON metadata files containing patch details such as label statistics, pixel counts, and image properties.






# Patch Reconstructor
## Overview
`patches_to_nifti.py` is a Python script designed to reconstruct NIfTI volumes from extracted 2D patches. The script takes metadata files associated with each patch, places them in the correct spatial orientation, and saves them as NIfTI volumes. This tool is useful for converting subdivided patches back into their full volume forms after they have been classified or processed for machine learning or other research applications.

The script ensures that each patch is correctly oriented according to the metadata, and generates a NIfTI file for each reconstructed volume.

## Features
- Reconstructs 3D NIfTI volumes from 2D patches.

- Reads patch metadata (stored in JSON format) to correctly place patches in the volume.

- Supports different planes: axial (z), sagittal (x), and coronal (y).

- Saves reconstructed volumes as `.nii.gz` files, compatible with medical imaging tools.

- Handles metadata to track the original patch slice and position within the volume.


## Requirements
To run `patches_to_nifti.py`, you need the following dependencies:

```python
pip install nibabel numpy matplotlib
```

## Imported Packages
The script uses the following Python libraries:

```python
import os
import json
import numpy as np
import nibabel as nib
import matplotlib.pyplot as plt
from glob import glob
```

## Usage
Reconstructing Volumes
Run the script using the following function:

```python
from patches_to_nifti import reconstruct_volume

reconstruct_volume(
    patches_dir="output_patches_brain_x",   # Directory containing the extracted patches and metadata
    output_dir="reconstructed_volumes_x"    # Directory to save the reconstructed NIfTI volumes
)
```

## Example Usage

```python

# Reconstructing volumes for axial plane (z)
reconstruct_volume("output_patches_brain_z", "reconstructed_volumes_z")

# Reconstructing volumes for sagittal plane (x)
reconstruct_volume("output_patches_brain_x", "reconstructed_volumes_x")

# Reconstructing volumes for coronal plane (y)
reconstruct_volume("output_patches_brain_y", "reconstructed_volumes_y")
```


## Parameters
- `patches_dir`: Directory containing the patches and metadata files to reconstruct volumes from.

- `output_dir`: Directory where the reconstructed NIfTI volumes will be saved.


## Output
The reconstructed volumes are saved in the specified output directory (`output_dir`). Each volume is stored in NIfTI format (.nii.gz) with the following details:

- Reconstructed NIfTI files, one per patch slice.
- Affine matrix is set to an identity matrix (placeholder).
- Metadata information from each patch is used to determine correct slice orientation and position in the volume.



# Demo
<object data="https://github.com/NeuroADaS-Lab/NeuroADaS_patches/blob/main/DEMO_of_patch_extraction_and_reconstruction.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="https://github.com/NeuroADaS-Lab/NeuroADaS_patches/blob/main/DEMO_of_patch_extraction_and_reconstruction.pdf">
        <p>Please view the PDF demo: <a href="https://github.com/NeuroADaS-Lab/NeuroADaS_patches/blob/main/DEMO_of_patch_extraction_and_reconstruction.pdf">View PDF</a>.</p>
    </embed>
</object>

