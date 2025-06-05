---
date:
  created: 2025-02-21
authors:
  - taddyb
---

# Coding Blog: How to access NWM forcings using VirtualiZarr

One of the most underrated aspects of NOAA's weather data is how much data is published on a daily basis. The National Water Model (Cosgrove et al. 2024) produces nine total operational configurations, each with a different look back peroid, forecast range, and domain extent. The number of .nc files output to S3 Object Storage is in the tens of thousands... per day!

While this amount of data is monumental for machine learning, or other hydrological analysis, it's cumbersome to read every .nc file individually, or download this amount of data to disk. That is where [VirtualiZarr](https://github.com/zarr-developers/VirtualiZarr) comes into play as it allows existing .nc files / structures to be viewed as zarr stores/xarray datasets without having to duplicate any data!

Below is a tutorial on how you can use VirtualiZarr to read a forecast from the National Water Model as a singular zarr store for your own studies

<!-- more -->

### Installing dependencies
The following dependencies will be used, with a version of Python >=3.11
```
dask==2025.2.0
distributed==2025.2.0
xarray==2025.1.2
s3fs==2025.2.0
virtualizarr==1.3.1
h5py==3.13.0
cubed-xarray==0.0.7
h5netcdf==1.5.0
```

You can either put these into a `requirements.txt` file, or install them one by one via your package manager.

### Figure out what data you'll be using from the National Water Model
Given how much data exists out there from the national water model, you'll have to be explicit as to what you're trying to access. You can check out the
[National Water Model Bucket](https://noaa-nwm-pds.s3.amazonaws.com/index.html) yourself, but the following information needs to be known:

- `date`
    - The datetime we're reading
    - ex: `20250216`
- `forecast_type`
    - The forecast issued by the National Water Model
    - ex: `short_range`
- `initial_time`
    - The time at which the forecast was issued (In Zulu time)
    - ex: `t00z`
- `variable`
    - The variable we're looking to read
    - ex: `channel_rt`

### Query your files

Since this data reads from the cloud, we'll need to create our paths to s3. Below shows how we'll be creating the URLs to our datasets dynamically.

```python
import fsspec

fs = fsspec.filesystem("s3", anon=True)
file_pattern = f"s3://noaa-nwm-pds/nwm.{date}/{forecast_type}/nwm.{initial_time}.{forecast_type}.{variable}.*.nc"
noaa_files = fs.glob(file_pattern)
noaa_files = sorted(["s3://" + f for f in noaa_files])

so = dict(anon=True, default_fill_cache=False, default_cache_type="none")
```

### Read your stores from the cloud

Now that our s3 links are created, let's spin up a Dask Cluster to read our files from the cloud in parallel

```python
import xarray as xr
from virtualizarr import open_virtual_dataset
from dask.distributed import LocalCluster

def _process(url: str, so: dict[str, str]):
    vds = open_virtual_dataset(
            url, 
            drop_variables="crs",
            indexes={}, 
            reader_options={"storage_options": so}
        )
    return vds

fs = fsspec.filesystem("s3", anon=True)
client_settings: dict[str, int | str] = {
    "n_workers":9,
    "memory_limit":"2GiB",
}

cluster= LocalCluster(**client_settings)  
client = cluster.get_client()

so = dict(anon=True, default_fill_cache=False, default_cache_type="none")

futures = []
for url in noaa_files:  
    future = client.submit(_process, url, so)
    futures.append(future)

virtual_datasets = client.gather(futures)  
```

*Notice:* How we drop the CRS from these netcdf files as their string byte-type throws an error

### Build your VirtualiZarr store

Now that our virtual datasets are created, let's create the virtual reference and save the output locally to a kerchunk json file.

```python
virtual_datasets[0].virtualize.to_kerchunk("NWM.json", format="json")
```

Outputs can be read through thefollowing:
```python
import s3fs
import os

os.environ['AWS_REGION'] = 'us-east-1'
fs = s3fs.S3FileSystem(anon=True, client_kwargs={'region_name': 'us-east-1'})
storage_options = dict(
    remote_protocol="s3", remote_options=dict(anon=True)
)
ds = xr.open_dataset(
    "NWM_ACCET.json",
    engine="kerchunk",
    backend_kwargs={"storage_options": storage_options},
)
ds.ACCET.plot(vmin=-1, vmax=ds.ACCET.max().values)
```

<p align="center">
  <img src="https://github.com/DeepGroundwater/DeepGroundwater.github.io/blob/master/docs/blog/posts/pics/nwm_accet.png?raw=true" alt="Accumulated Total ET" width="500"/>
  <br>
  <em>Figure 1: Plotted Accumlated total ET for CONUS</em>
</p>

That's it! You now how a collection of many .nc files in one xarray dataset that can be read into your hydrologic analysis.

For the full demo, and code package we put together, check out our demo repo at: [DeepGroundwater NWM Batcher](https://github.com/DeepGroundwater/nwm_batcher/blob/master/examples/read_short_range.ipynb). With a snippet below:

```python
virtual_datasets = nwm_batcher.read(
    date="20250516",
    forecast_type="short_range",
    initial_time="t00z",
    variable="land",
    data_variable="ACCET",
    coordinates=["time", "reference_time", "x", "y"]
)
```
