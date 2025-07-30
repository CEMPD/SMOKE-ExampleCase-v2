## Processing Biogenic Emissions

First, source the directory definitions script:


```
> source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

Each EPA's modeling platform typically includes normalized emissions calculated by Normbeis4 for the continental United States at both 12- and 36-kilometer resolutions. This ExampleCase package also includes the normalized emission file from Normbeis4 for the 12LISTOS domain. This normalized emission is applicable for the whole year and thus don't need to be processed for the individual days in the episode. 

The normalized emissions file is located in the intermediate data directory for the beis4 sector:

```
> cd $IMD_ROOT/beis4
```

```
> ls
```

```
beis_norm_emis_12LISTOS_2018gg_18j.ncf
```

Switch into the biogenics run scripts directory:

```
> cd $CASESCRIPTS/biogenics
```


Run the script for the beis4 sector:

```
> ./Annual_beis4_12US1_2018gg_18j.csh
```

The model-ready biogenic emissions files are located in the beis4 pre-merged directory. Unlike other sectors, these files were created by Tmpbeis4 instead of Smkmerge. Tmpbeis4 creates both outputs for both mass- and mole-based units.

```
> cd $CASE_ROOT/premerged/beis4
```

```
> ls emis_mass*
```

```
emis_mass_beis4_20180801_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mass_beis4_20180802_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mass_beis4_20180803_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
...
emis_mass_beis4_20180830_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mass_beis4_20180831_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
> ls emis_mole*

emis_mole_beis4_20180801_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_beis4_20180802_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_beis4_20180803_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
...
emis_mole_beis4_20180830_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_beis4_20180831_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
```
 
