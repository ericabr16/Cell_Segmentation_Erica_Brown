# BAL Cytology Image Segmentation Benchmark
## Project Overview
This repository contains a technical evaluation of the Cellpose (v4.0.1) segmentation model applied to the BBBC018 Bronchoalveolar Lavage (BAL) cytology dataset. 

The primary goal was to establish a robust benchmarking pipeline that accurately handles high cell confluency and overlapping cellular boundaries.

# Technical Challenges & Solutions
## 1. Instance Separation in Confluent Clusters
Challenge: The provided ground truth masks were 1-pixel wide outlines. Standard binary filling caused individual cells in clusters to merge into single objects, leading to inaccurate IoU metrics.
Solution: Implemented a Nuclei-Seeded Watershed Transformation. By using the Nuclei to identify unique markers (seeds) and the Actin to define cellular boundaries, I successfully recovered individual cell instances from the outline data.

## 2. Coordinate Alignment
Challenge: Discrepancy between the image coordinate system (.DIB files) and the mask coordinate system (.png files).
Solution: Applied a vertical flip (`np.flipud`) to the image data to ensure perfect spatial alignment for benchmarking.

# Metrics
The model was evaluated using the Mean Intersection over Union (mIoU).
* Ground Truth: Seeded Watershed (Nuclei + Actin)
* Prediction: Cellpose `cyto3` model
* Final Mean IoU: 0.4728

# Setup & Reproducibility
1. Clone the repository.
2. Install dependencies: `pip install -r requirements.txt`.
3. Place the BBBC018 images in the `/images` folder and outlines in the `/outlines` folder.
4. Run the Jupyter Notebook `BAL_Cell_Segmentation.ipynb`.

*Note: Inference was performed on a CPU. For 55 images, the process takes approximately 110 minutes. GPU acceleration is recommended for production scaling.
