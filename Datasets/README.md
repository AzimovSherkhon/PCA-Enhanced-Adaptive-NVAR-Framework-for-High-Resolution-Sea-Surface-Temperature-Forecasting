# SST data preparation

The regional experiments use sea-water potential temperature (`thetao`) from the Copernicus Marine Service product:

```text
cmems_mod_glo_phy_my_0.083deg_P1D-m
```

The current dataset notebooks request:

- variable: `thetao`
- surface/depth level: approximately `0.49402499 m`
- start date: `2012-08-19`
- end date: `2026-04-28`

## Regional subsets

| Region | Minimum longitude | Maximum longitude | Minimum latitude | Maximum latitude |
|---|---:|---:|---:|---:|
| East Sea | 129.0°E | 133.0°E | 35.0°N | 40.0°N |
| Yellow Sea | 122.5°E | 126.0°E | 34.0°N | 38.0°N |
| East China Sea | 124.0°E | 128.0°E | 28.0°N | 33.0°N |

These values are taken from the corresponding dataset-acquisition notebooks in `Datasets/`.

## Step 1 — obtain the NetCDF data

Open the relevant notebook:

```text
Datasets/East_Sea_Dataset.ipynb
Datasets/Yellow_Sea_Dataset.ipynb
Datasets/East_China_Sea_Dataset.ipynb
```

The notebooks use `copernicusmarine.subset()` with the dataset identifier above.

The resulting NetCDF file is then opened with `xarray`.

## Step 2 — create the regional NumPy array

The notebooks:

1. read `thetao`;
2. remove the depth dimension;
3. construct an ocean mask from the first time step;
4. remove invalid/land grid cells;
5. arrange the data as `(time, spatial features)`;
6. transpose to `(spatial features, time)`;
7. save the result as a NumPy array.

The corrected filename mapping is:

```text
East Sea       -> east_sea_sst.npy
Yellow Sea     -> yellow_sea_sst.npy
East China Sea -> east_china_sea_sst.npy
```

## Step 3 — normalize the data

Each regional normalization notebook uses:

```text
warmup = 100
training = 3900
validation = 500
test = 500
total = 5000
```

The normalization statistics are calculated from the first:

```text
100 + 3900 = 4000
```

time steps:

```python
X_train = data_marine[:, 0:4000]
mean = X_train.mean(axis=1, keepdims=True)
std = X_train.std(axis=1, keepdims=True)

data_norm = (data_marine - mean) / std
```

The normalized array is saved as:

```text
sst_normalized_5k.npy
```

Because each regional experiment has its own directory, the same normalized filename can be used independently:

```text
East Sea/sst_normalized_5k.npy
Yellow Sea/sst_normalized_5k.npy
East China Sea/sst_normalized_5k.npy
```

## Step 4 — run the forecasting notebook

After normalization, open the corresponding regional notebook:

```text
East Sea/Adaptive_NVAR_East_Sea.ipynb
Yellow Sea/Adaptive_NVAR_Yellow_Sea.ipynb
East China Sea/Adaptive_NVAR_East_China_Sea.ipynb
```

The forecasting notebooks load:

```python
np.load("sst_normalized_5k.npy")
```

Therefore, when running a regional notebook, the normalized file should be available in that notebook's working directory.

## Data redistribution

The original oceanographic NetCDF files are not included in this repository. Users should obtain the data from the Copernicus Marine Service according to its access and licensing terms.

This repository contains the acquisition/preprocessing notebooks needed to create the regional arrays used by the forecasting notebooks.
