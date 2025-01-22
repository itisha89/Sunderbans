# README: Vegetation Indices Analysis in Sundarbans Using Sentinel-2 Data

## Overview

This project utilizes satellite data from the Sentinel-2 mission to analyze vegetation coverage in the Sundarbans region using various vegetation indices, such as NDVI, RENDVI, and EVI. The Sentinel-2 data for this analysis was acquired on 27th January 2020 and contains multiple spectral bands, including Red, NIR, Blue, and Red-Edge. These indices are useful for assessing vegetation health, identifying water bodies, and understanding forest coverage in the area.

## Objective

The objective of this project is to explore satellite images of a selected zone within the Sundarbans range and calculate vegetation indices to assess forest coverage and vegetation health. The vegetation indices calculated in this project include:
1. **NDVI (Normalized Difference Vegetation Index)**
2. **RENDVI (Red-Edge Normalized Difference Vegetation Index)**
3. **EVI (Enhanced Vegetation Index)**

## Data

The data used in this project was obtained from the **Sentinel-2** satellite and consists of 12 spectral bands. These bands are represented in `.tiff` files and contain information about various wavelengths of light captured by the satellite. The dataset used here covers the date **27th January 2020**.

### Bands Used:
- **Band 1**: Coastal Aerosol
- **Band 2**: Blue
- **Band 3**: Green
- **Band 4**: Red
- **Band 5**: Red Edge
- **Band 6**: Red Edge
- **Band 7**: Red Edge
- **Band 8**: Near Infrared (NIR)
- **Band 8A**: Narrow Near Infrared
- **Band 9**: Water Vapour
- **Band 10**: SWIR (Shortwave Infrared)
- **Band 11**: SWIR
- **Band 12**: SWIR

![image](https://github.com/user-attachments/assets/1359459b-1d5f-4795-8ef9-ea188433f1cc)

## Requirements

1. **Python 3.x**
2. **Libraries**:
   - `earthpy`: for working with Earth observation data
   - `rasterio`: for reading and processing raster data
   - `matplotlib`: for plotting images and visualizations
   - `numpy`: for numerical operations

## Installation

To set up the environment for this project, run the following commands:

1. Clone the repository:
    ```bash
    git clone https://github.com/itisha89/Sunderbans.git
    ```

2. Install required packages:
    ```bash
    pip install earthpy rasterio matplotlib numpy
    ```

## Usage

### Step 1: Load the Sentinel-2 Bands

The first step is to load the satellite data into Python. The `.tiff` files are read using the `rasterio` library.

```python
import glob as gb
import rasterio as rio

sunderbans_paths = gb.glob("/content/Sunderbans/*B*.tiff")
band1_path = '/content/Sunderbans/2020-01-27-00_00_2020-01-27-23_59_Sentinel-2_L2A_B01_(Raw).tiff'
band1 = rio.open(band1_path)
```

![image](https://github.com/user-attachments/assets/f4fa7dd0-8d08-4a0d-98a5-009a5d44c820)


### Step 2: Plot the Bands

After loading the bands, you can visualize the image by plotting the bands. For RGB visualization, bands 4 (Red), 3 (Green), and 2 (Blue) are used.

```python
import earthpy.plot as epp
import matplotlib.pyplot as plt

epp.plot_rgb(arr_st, rgb=(3, 2, 1))
plt.show()
```

![image](https://github.com/user-attachments/assets/3ad14c67-c974-49a3-abc2-e754646a0222)


### Step 3: Calculate Vegetation Indices

#### NDVI (Normalized Difference Vegetation Index)

NDVI is calculated using the formula:

$$\text{NDVI} = \frac{\text{(NIR - Red)}}{\text{(NIR + Red)}}$$

```python
import earthpy.spatial as es

ndvi = es.normalized_diff(arr_st[7], arr_st[3])
epp.plot_bands(ndvi, cmap="RdYlGn", cols=1, vmin=-1, vmax=1)
plt.show()
```

![image](https://github.com/user-attachments/assets/8e6f3d06-0748-48e0-918e-546fe2fec5a9)

Here you can observe:

The area shaded in light green color represents light vegetation.

The area shaded in dark green color represents dense vegetation.

The water bodies are marked using a distinctively a darker color out of the palettes  +1  value.

#### RENDVI (Red-Edge Normalized Difference Vegetation Index)

RENDVI is used to measure chlorophyll content in plants and is calculated using the formula:

$$\text{RENDVI} = \frac{\text{(NIR - RE)}}{\text{(NIR + RE)}}$$

```python
rendvi = es.normalized_diff(arr_st[7], arr_st[4])
epp.plot_bands(rendvi, cmap="RdYlGn", cols=1, vmin=-1, vmax=1)
plt.show()
```
![image](https://github.com/user-attachments/assets/24c858d7-2ecb-4938-8e4b-b008d2f1c910)

Here, we observe that the chlorophyll distribution is a bit different when compared with NDVI as it is highly dependent on:

The season and climatic conditions

Plant maturity

Type of the plant.

A general practice is to analyse the RENDVI at scientifically identified seasons to extract meaningful information on the crop health


#### EVI (Enhanced Vegetation Index)

EVI is an optimized index that improves NDVI's accuracy by incorporating the Blue band. It is calculated as:

$$\text{EVI} = \frac{2.5 \times (\text{(NIR} - \text{Red)})}{\text{[NIR} + (6.0 \times \text{Red}) - (7.5 \times \text{Blue}) + 1.0]}$$

```python
evi = 2.5 * (arr_st[7] - arr_st[3]) / (arr_st[7] + (6.0 * arr_st[3]) - (7.5 * arr_st[1]) + 1.0)
epp.plot_bands(evi, cmap="RdYlGn", cols=1, vmin=-1, vmax=1)
plt.show()
```

Here we observe, that water bodies are clearly marked in red color.

Advantages of EVI:

It enables us to clearly identify smaller water bodies. This is evident from the above plot.

Due to improved sensitivity the variation in density in lighter vegetation areas can be identified easily.![image](https://github.com/user-attachments/assets/640232ec-324a-4882-a0c6-64402bde4c65)


### Step 4: Interpret the Results

- **NDVI**: Light green indicates light vegetation, and dark green represents dense vegetation. Water bodies appear dark.
- **RENDVI**: Helps to assess chlorophyll content, giving insight into plant health.
- **EVI**: Highlights smaller water bodies and offers better clarity in dense vegetation areas.

## Conclusion

The vegetation indices computed from the Sentinel-2 satellite data provide valuable insights into vegetation health, forest coverage, and land-water distribution in the Sundarbans region. NDVI, RENDVI, and EVI each offer distinct advantages and can be used together for more comprehensive vegetation analysis.
