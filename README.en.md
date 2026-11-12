[简体中文](README.md) | **English**

# Wind Turbine Tower Crack Dataset

A UAV image dataset of concrete wind turbine towers for grid-based crack classification and recognition. This repository provides dataset documentation, label conventions, the related paper, and a download link.

**[Download dataset (Baidu Netdisk)](https://pan.baidu.com/s/1yheaGH2gGWVz1JL7D6NhmA?pwd=bsyx)** · Access code: `bsyx`  
**[Read the paper](https://doi.org/10.1177/13694332251344664)** · [BibTeX citation](citation.bib)

## Download

The shared folder is named “风电塔筒” (wind turbine towers).

- Link: https://pan.baidu.com/s/1yheaGH2gGWVz1JL7D6NhmA?pwd=bsyx
- Access code: `bsyx`

Images are distributed through Baidu Netdisk. This repository contains documentation and the paper citation.

## Dataset overview

The prepared `data_split` collection contains images and matching label files:

| Item | Count / specification |
| --- | --- |
| Images | 3,792 RGB JPG images |
| Image size | 341 × 227 pixels |
| Labels | 3,792 TXT files |
| Crack-positive grid annotations | 77,673 rows |
| Original image size | 5456 × 3632 pixels |
| Original image partition | 16 × 16 tiles |

The **16 × 16 partition divides each original image into image tiles**. Label rows describe crack-positive annotation grid cells within each tile.

### Directory structure

```text
data_split/
├── images/
│   ├── 119MSDCF_DSC09765_4_4.JPG
│   └── ...
├── labels/
│   ├── 119MSDCF_DSC09765_4_4.txt
│   └── ...
└── README.md
```

Each image is paired with a label file sharing the same filename stem.

### Filenames and cropping

```text
original_image_name_row_column.JPG
```

The final two numbers are the row `r` and column `c`, both zero-based and ranging from 0 to 15. Each original image is partitioned into 16 × 16 tiles. Crop coordinates are:

```text
left   = c * 341
top    = r * 227
right  = (c + 1) * 341
bottom = (r + 1) * 227
```

The right and bottom boundaries are exclusive. Only tiles with corresponding existing labels are retained; the collection does not include all 256 tiles from every original image.

## Label conventions: grid classification

**`1` means that the grid cell contains a crack. Only crack-positive cells are listed; cells without cracks have no separate annotation row.**

Each TXT file corresponds to one image tile. Each row contains five space-separated values:

```text
1 x_center y_center width height
```

| Value | Meaning |
| --- | --- |
| `1` | Crack-present indicator for the cell |
| `x_center` | Cell center x-coordinate / image tile width |
| `y_center` | Cell center y-coordinate / image tile height |
| `width` | Cell width / image tile width |
| `height` | Cell height / image tile height |

The last four values are normalized relative to the current **341 × 227 image tile**. Multiple rows describe multiple crack-positive grid cells, rather than separate crack instances or pixel-level crack contours.

Example annotation:

```text
1 0.5 0.5526315789473685 0.03225806451612903 0.05263157894736842
```

Convert normalized cell coordinates to pixel boundaries:

```python
W, H = 341, 227
left   = (x_center - width / 2) * W
top    = (y_center - height / 2) * H
right  = (x_center + width / 2) * W
bottom = (y_center + height / 2) * H
```

The original label contents and the indicator `1` are preserved.

## Related paper

**Concrete wind turbine tower crack assessment based on drone imaging using computer vision and artificial intelligence**

Bao-Luo Li, Chu-Qiao Feng, Si-Hang Wei, Yu-Fei Liu.  
*Advances in Structural Engineering*, 2025.  
DOI: [10.1177/13694332251344664](https://doi.org/10.1177/13694332251344664)

### Research overview

The paper presents a UAV-based crack assessment workflow for concrete wind turbine towers:

1. **UAV image acquisition:** flight trajectories and photography strategies suited to tall towers, balancing fine-crack imaging and overlap between adjacent images.
2. **Grid-based crack recognition:** an attention-enhanced grid-based convolutional neural network identifies crack regions, followed by digital image processing to extract fine cracks.
3. **3D reconstruction:** structure from motion (SfM) and surface reconstruction generate a triangular mesh model of the tower.
4. **Incremental crack projection:** 2D crack information is mapped onto the 3D surface while handling overlapping image regions to reduce duplicate crack counting.
5. **Quantification and assessment:** the spatial model supports crack localization and parameter measurement for structural maintenance.

### Citation

Please cite the related paper when using the data and research methods:

```bibtex
@article{Li2025WindTowerCrack,
  author = {Li, Bao-Luo and Feng, Chu-Qiao and Wei, Si-Hang and Liu, Yu-Fei},
  title = {Concrete wind turbine tower crack assessment based on drone imaging using computer vision and artificial intelligence},
  journal = {Advances in Structural Engineering},
  year = {2025},
  doi = {10.1177/13694332251344664},
  url = {https://doi.org/10.1177/13694332251344664}
}
```
