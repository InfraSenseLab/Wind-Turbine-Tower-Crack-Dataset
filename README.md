# Wind Turbine Tower Crack Dataset
# 风电塔筒裂缝数据集

面向无人机巡检场景的混凝土风电塔筒裂缝图像数据集，用于裂缝网格分类与识别研究。本仓库提供数据集介绍、标签规则、相关论文及数据下载入口。

**[下载数据集（百度网盘）](https://pan.baidu.com/s/1yheaGH2gGWVz1JL7D6NhmA?pwd=bsyx)** · 提取码：`bsyx`  
**[阅读论文](https://doi.org/10.1177/13694332251344664)** · [BibTeX 引用](citation.bib)

## 数据下载

通过百度网盘获取分享文件夹“风电塔筒”：

- 链接：https://pan.baidu.com/s/1yheaGH2gGWVz1JL7D6NhmA?pwd=bsyx
- 提取码：`bsyx`

图像数据通过网盘分发，本仓库保存介绍文档和论文引用。

## 数据集概况

本次整理的小图数据集为 `data_split`，包含同名对应的图像与标签：

| 项目 | 数量 / 规格 |
| --- | --- |
| 小图 | 3,792 张 RGB JPG |
| 小图尺寸 | 341 × 227 像素 |
| 标签 | 3,792 个 TXT 文件 |
| 有裂缝网格标注 | 77,673 行 |
| 原图尺寸 | 5456 × 3632 像素 |
| 原图切分方式 | 16 × 16 网格 |

这里的 **16 × 16 是原图到小图的切分网格**；标签行描述的是每张小图内部有裂缝的标注网格。

**版本说明：**论文描述的实验数据集包含 3,750 张小图，按 6:2:2 划分为训练、验证、测试集（2,250 / 750 / 750）。本次根据现有标签恢复并整理得到 3,792 张小图，数量以本次整理结果为准；不要将其直接视为论文实验划分。

### 目录结构

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

图像与标签通过不含扩展名的文件名一一匹配。

### 图片命名与裁剪规则

```text
原图名称_行号_列号.JPG
```

末尾两个数字分别为行号 `r`、列号 `c`，均从 0 开始，范围为 0–15。原图按 16 × 16 切分，每张小图的裁剪范围为：

```text
left   = c * 341
top    = r * 227
right  = (c + 1) * 341
bottom = (r + 1) * 227
```

右、下边界不包含在裁剪区域内。本次仅保留现有标签对应的小图，并非每张原图的全部 256 个切块。

## 标签规则：网格分类

**`1` 表示该网格有裂缝。标签只记录有裂缝的网格，没有裂缝的网格不单独列出标签行。**

每个 TXT 文件对应一张小图，每行以空格分隔 5 列：

```text
1 x_center y_center width height
```

| 列 | 含义 |
| --- | --- |
| `1` | 网格有裂缝的标记 |
| `x_center` | 网格中心横坐标 / 当前小图宽度 |
| `y_center` | 网格中心纵坐标 / 当前小图高度 |
| `width` | 网格宽度 / 当前小图宽度 |
| `height` | 网格高度 / 当前小图高度 |

后四列为归一化数值，参考尺寸是当前 **341 × 227 小图**。一个文件的多行表示多个有裂缝网格，不代表多个独立裂缝实例，也不是像素级裂缝轮廓。

实际示例：

```text
1 0.5 0.5526315789473685 0.03225806451612903 0.05263157894736842
```

换算为小图中的像素边界：

```python
W, H = 341, 227
left   = (x_center - width / 2) * W
top    = (y_center - height / 2) * H
right  = (x_center + width / 2) * W
bottom = (y_center + height / 2) * H
```

本次整理保留了源标签的全部内容和标记 `1`。

## 相关论文

**Concrete wind turbine tower crack assessment based on drone imaging using computer vision and artificial intelligence**

Bao-Luo Li, Chu-Qiao Feng, Si-Hang Wei, Yu-Fei Liu.  
*Advances in Structural Engineering*, 2025.  
DOI：[10.1177/13694332251344664](https://doi.org/10.1177/13694332251344664)

### 研究内容

论文提出面向混凝土风电塔筒的无人机裂缝评估流程：

1. **无人机图像采集**：设计适合高耸塔筒的飞行轨迹与拍摄策略，兼顾细裂缝成像和相邻图像重叠。
2. **裂缝网格识别**：使用注意力增强的网格分类卷积神经网络定位有裂缝区域，再结合数字图像处理提取细裂缝。
3. **三维重建**：通过运动恢复结构（SfM）及表面重建生成塔筒三角网格模型。
4. **增量裂缝投影**：将二维裂缝信息映射到三维表面，处理相邻图像的重叠区域，减少裂缝重复计数。
5. **参数量化与评估**：结合空间模型开展裂缝定位和参数计算，为结构维护提供依据。

论文表 3 报告所提网络的 AP 为 0.92、Dice 为 0.83。上述数值为论文实验结果，不是本仓库对整理后数据集重新训练或评测的结果。

### 引用

使用相关数据和研究方法时，请引用：

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

## English overview

This repository documents a UAV image dataset for crack assessment of concrete wind turbine towers and links to the related 2025 paper. The prepared `data_split` collection contains **3,792 RGB images (341 × 227 pixels)** and matching text labels. Each label row describes a crack-positive grid cell: **1 means crack present**, followed by normalized cell center coordinates and cell size.

The paper reports an experimental dataset of 3,750 images; the prepared collection documented here contains 3,792 images recovered from the available labels. These counts and splits should be distinguished.

**Download:** [Baidu Netdisk](https://pan.baidu.com/s/1yheaGH2gGWVz1JL7D6NhmA?pwd=bsyx), access code: `bsyx`.  
**Paper:** [DOI: 10.1177/13694332251344664](https://doi.org/10.1177/13694332251344664).
