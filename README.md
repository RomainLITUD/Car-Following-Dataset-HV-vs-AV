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
