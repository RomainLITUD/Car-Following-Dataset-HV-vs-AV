# Large Car-following Dataset Based on Lyft level-5: Following Autonomous Vehicles vs. Human-driven Vehicles

- This is the description of the comparisive car-following dataset for studying driving behaviours when following AVs vs. HVs. The relevant paper is available via this link: [manuscript (click)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4241523). This paper describes how the dataset is prepared.

## Quick start

### Dataset download

* The dataset is available by 4.TU research dataset repository. Click the link to download the dataset: [download page (click)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4241523).

* The dataset contains the following files:
``` bash
 - trainHH.zarr       # HV following HV pairs from the training set
 - trainHA.zarr       # HV following AV pairs from the training set
 - valHH.zarr         # HV following HV pairs from the validation set
 - valHA.zarr         # HV following AV pairs from the validation set
 - regime_trainHH.csv # Regime of each pairs
 - regime_trainHA.csv # Regime of each pairs
```

### Requirements

* The dataset is organized in a compact `.zarr` form that allows directly read data from the disk.

* Only the following python packages are required:
``` bash
zarr
numpy
pandas
```

### Read car-following data.

* To select the n-th car-following pairs from a specific subset, use the following code:
``` bash
data = zarr.open('./trainHH.zarr', mode='r')
start, end = data.index_range[n]

# get vehicle size
size_lead = 4.85              # this is for AV
size_lead = data.lead_size[n] # this is for HV
size_follow = data.follow_size[n]

# get timestamps
timestamps = data.timestamp[start:end]

# get position, speed, and acceleration
x_lead = data.lead_centroid[start:end]
v_lead = data.lead_velocity[start:end]
a_lead = data.lead_acceleration[start:end]

x_follow = data.follow_centroid[start:end]
v_follow = data.follow_velocity[start:end]
a_follow = data.follow_acceleration[start:end]
```

* Read regime information

-the two `.csv` file list the following information:

| case_id | A | C | D | F | Fa | Fd | S |regime_comb |
|     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |
|48 | 4.3 |15.7 | 0.2 |19.6 | 6.5 | 0.6 | 1.2 | FaCADFSFd |

- A, C, D, F, Fa, Fd, S are 6 basic car-following regimes decribed in our paper manuscript. The `.csv` gives their durations in second for each car-following case id. The `regime_comb` gives how many regimes contained in each case, which can be used for selecting the desired regimes.
