## Overal Instructions

Similarly to structure of EPA Emission Modeling Platform (EMP), in this SMOKE Example Case this directory contains run scripts 
for onroad onroad mobile source emissions. These are emissions created by vehicles that drive on roads. Unlike other sectors which start from estimates of emissions, this processing starts with activity data, which gets combined with emission factors to calculate emissions.

First, source the directory definitions script:


```
> source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

Switch to the onroad input directory to take a look at the activity data that gets used for the onroad mobile sources.

```
> cd $CASEINPUTS/onroad/
```

```
> ls
```

```
HOTELING_2018gc_23feb2021_nf_v1.csv
ONI_IDLING_2018gc_MOVES3_23feb2021_nf_v1.csv
SPEED_2017NEI_from_CDBs_MOVES3_fuels_03may2021_v8.csv
STARTS_2018gc_MOVES3_23feb2021_nf_v1.csv
VMT_2018gc_MOVES3_23feb2021_nf_v1.csv
VPOP_2018gc_MOVES3_23feb2021_nf_v1.csv
eftables
```


The `eftables` directory contains the emission factors created by MOVES. This Example Case only uses emission factors for a single state (New York), with seven representative counties within that state, and a single representative month (7 = summer).

```
> ls eftables
```

```
rateperdistance_smoke_nata_20210219_2018-20210208_36001_7.csv
rateperdistance_smoke_nata_20210219_2018-20210208_36007_7.csv
rateperdistance_smoke_nata_20210219_2018-20210208_36065_7.csv
rateperdistance_smoke_nata_20210219_2018-20210208_36081_7.csv
rateperdistance_smoke_nata_20210219_2018-20210208_36103_7.csv
rateperdistance_smoke_nata_20210219_2018-20210208_36111_7.csv
rateperdistance_smoke_nata_20210219_2018-20210208_36119_7.csv
rateperhour_smoke_nata_20210219_2018-20210208_36001_7.csv
rateperhour_smoke_nata_20210219_2018-20210208_36007_7.csv
rateperhour_smoke_nata_20210219_2018-20210208_36065_7.csv
...
rateperhouroni_smoke_nata_20210219_2018-20210208_36001_7.csv
rateperhouroni_smoke_nata_20210219_2018-20210208_36007_7.csv
rateperhouroni_smoke_nata_20210219_2018-20210208_36065_7.csv
...
rateperprofile_smoke_nata_20210219_2018-20210208_36001_07.csv
rateperprofile_smoke_nata_20210219_2018-20210208_36007_07.csv
rateperprofile_smoke_nata_20210219_2018-20210208_36065_07.csv
...
rateperstart_smoke_nata_20210219_2018-20210208_36001_7.csv
rateperstart_smoke_nata_20210219_2018-20210208_36007_7.csv
rateperstart_smoke_nata_20210219_2018-20210208_36065_7.csv
...
ratepervehicle_smoke_nata_20210219_2018-20210208_36001_7.csv
ratepervehicle_smoke_nata_20210219_2018-20210208_36007_7.csv
ratepervehicle_smoke_nata_20210219_2018-20210208_36065_7.csv
...
```

For a given county (e.g. 36001), each file contains emission factors for a different mode.

```
> ls eftables/*36001*
```

```
eftables/rateperdistance_smoke_nata_20210219_2018-20210208_36001_7.csv
eftables/rateperhour_smoke_nata_20210219_2018-20210208_36001_7.csv
eftables/rateperhouroni_smoke_nata_20210219_2018-20210208_36001_7.csv
eftables/rateperprofile_smoke_nata_20210219_2018-20210208_36001_07.csv
eftables/rateperstart_smoke_nata_20210219_2018-20210208_36001_7.csv
eftables/ratepervehicle_smoke_nata_20210219_2018-20210208_36001_7.csv
```

For instance, the rate-per-distance (RPD) factors estimate emissions produced while vehicles are driving on roads, such as running exhaust and brakewear. The RPD factors are given in grams-per-vehicle-mile and are combined with vehicle miles traveled (VMT) activity data.

The VMT activity data is stored in the activity inventory file named `VMT_2018gc_MOVES3_23feb2021_nf_v1.csv`. This file lists VMT for each county, broken down by fuel type, vehicle type, and road type.

The other activity inventory files in the `$CASEINPUTS/onroad/` directory contain the following types of activity data:

+ HOTELING_2018gc_23feb2021_nf_v1.csv
    - Extended idling by heavy-duty diesel truck (hotelling hours)
    - Used with rate-per-hour (RPH) emission factors
+ ONI_IDLING_2018gc_MOVES3_23feb2021_nf_v1.csv
    - Off-network idling
    - Used with rate-per-hour-oni (RPHO) emission factors
+ SPEED_2017NEI_from_CDBs_MOVES3_fuels_03may2021_v8.csv
    - Average speeds
    - Used with rate-per-distance (RPD) emission factors
+ STARTS_2018gc_MOVES3_23feb2021_nf_v1.csv
    - Number of vehicle starts
    - Used with rate-per-start (RPS) emission factors
+ VMT_2018gc_MOVES3_23feb2021_nf_v1.csv
    - Vehicle miles traveled
    - Used with rate-per-distance (RPD) emission factors
+ VPOP_2018gc_MOVES3_23feb2021_nf_v1.csv
    - Vehicle population counts
    - Used with rate-per-profile (RPP) and rate-per-vehicle (RPV) emission factors

Next, lets look at the run scripts used for onroad mobile sources.

```
> cd $CASESCRIPTS/onroad
```

```
> ls
```

```
Monthly_onroad_RPD_daily_12US1_2018gg_18j.csh
Monthly_onroad_RPH_daily_12US1_2018gg_18j.csh
Monthly_onroad_RPHO_daily_12US1_2018gg_18j.csh
Monthly_onroad_RPP_daily_12US1_2018gg_18j.csh
Monthly_onroad_RPS_daily_12US1_2018gg_18j.csh
Monthly_onroad_RPV_daily_12US1_2018gg_18j.csh
Monthly_onroad_merge_daily_12US1_2018gg_18j.csh
```

The run scripts named Monthly_onroad_RP* each process one of the emission factor modes. These scripts need to run the following SMOKE programs:

+ Smkinven: read the appropriate activity inventory data for the mode
+ Spcmat: calculate factors to convert pollutants from the emission factors into model species
+ Grdmat: allocate county activity data to grid cells
+ Temporal: calculate hourly activity data
+ Smkreport: create reports summarizing the input activity data
+ Movesmrg: combine activity data with MOVES emission factors to create gridded, speciated, hourly emissions
+ Movesmrg will also create reports giving county total emissions by day

The run script Monthly_onroad_merge_daily_12US1_2018gg_18j.csh combines the results of the individual RP* scripts into one set of onroad emissions files.



## Example Case: Processing Rate-Per-Hour (RPH) *onroad* sector
Navigate to the onroad run scripts directory and run the RPH run script:

```
> cd $CASESCRIPTS/onroad/
```

```
./Monthly_onroad_RPH_daily_12US1_2018gg_18j.csh
```

The gridded, speciated, hourly emissions files created by Movesmrg are located in `$CASE_ROOT/premerged/onroad/RPH/`:

```
> ls $CASE_ROOT/premerged/onroad/RPH/
```

```
emis_mole_RPH_onroad_20180801_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_RPH_onroad_20180802_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
...
emis_mole_RPH_onroad_20180830_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_RPH_onroad_20180831_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
```


Reference Output Files for Comparison: [premerged_onroad.tar.gz](https://drive.google.com/file/d/18E_nKcmhmUWqLYPYxwxQbIWVfdcfTwRY/view?usp=share_link)
