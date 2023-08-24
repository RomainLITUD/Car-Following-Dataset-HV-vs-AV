# Large Car-following Dataset Based on Lyft level-5: Following Autonomous Vehicles vs. Human-driven Vehicles

- This is the description of the comparisive car-following dataset for studying driving behaviours when following AVs vs. HVs. The relevant paper is available via this link: [pre-printed(click)](https://arxiv.org/abs/2305.18921). This paper describes how the dataset is prepared.

## Quick start

### Dataset download

* The dataset is available by 4.TU research dataset repository. Click the link to download the dataset: [download page (click)](https://data.4tu.nl/datasets/1255994c-c64f-40f5-8121-9e952e308c9a/1). The download link may not be valid before July. You can also directly send an email to [g.li-5@tudelft.nl](g.li-5@tudelft.nl) to access the dataset.

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
python >=3.8
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

-The two `.csv` files list the following information:

| case_id | A | C | D | F | Fa | Fd | S |regime_comb |
|     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |     :---:      |
|48 | 4.3 |15.7 | 0.2 |19.6 | 6.5 | 0.6 | 1.2 | FaCADFSFd |

- A, C, D, F, Fa, Fd, and S are 6 basic car-following regimes described in our paper manuscript. The `.csv` gives their durations in seconds for each car-following case id. The `regime_comb` gives how many regimes are contained in each case, which can be used for selecting the desired regimes.

### Driver unique ids
In the HV-following-AV subset, different cases may have the same follower (human driver). They are separate because Lyft cut them into pieces. Here we reconstruct the unique human driver ids for every case. They are stored in two `.npz` files:

```
dr_train = np.load('driver_ids_train_HA.npz', allow_pickle = True)
dr_val = np.load('driver_ids_val_HA.npz', allow_pickle = True)

ids_train = dr_train['ids']
ids_val = dr_val['ids]
```

`ids_train` and `ids_val` are two 1D arrays whose lengths are the numbers of CF cases. The value gives the unique driver ID of the following HV of that case. One driver may appear in several sequential cases.
