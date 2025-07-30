## Merging Sectors

The final step in emissions processing is to merge the results from the individual sectors to create files ready for the air quality model. In this step, you'll merge the results from the sectors that you've processed previously.

First, let's look at the "sectorlist" file that the merging run script uses to determine which sectors to merge.

```
> source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

```
> cat $CASESCRIPTS/sectorlist_2018gg_training
```

```
sector,sectorcase,sectbaseyr,mrgapproach,prevyrspinup,endzip,speciation,mergesector,projectroot
"airports","2018gg_18j","2018","week_Y","SectBaseYr","N","","Y","<smoke_example_case>"
"beis4","2018gg_18j","2018","all","actualMet","N","","Y","<smoke_example_case>"
"nonroad","2018gg_18j","2018","mwdss_Y","SectBaseYr","N","","Y","<smoke_example_case>"
"np_solvents","2018gg_18j","2018","aveday_N","SectBaseYr","N","","Y","<smoke_example_case>"
"onroad","2018gg_18j","2018","all","SectBaseYr","N","","Y","<smoke_example_case>"
"pt_oilgas","2018gg_18j","2018","mwdss_Y","SectBaseYr","N","","N","<smoke_example_case>"
"ptegu","2018gg_18j","2018","all","SectBaseYr","N","","N","<smoke_example_case>"
"ptnonipm","2018gg_18j","2018","mwdss_Y","SectBaseYr","N","","N","<smoke_example_case>"
"rwc","2018gg_18j","2018","all","SectBaseYr","N","","Y","<smoke_example_case>"
```

NOTE: In the above *sectorlist* table, `<smoke_example_case>` is the path to smoke_example_case (e.g., `<directory where Example Case data was downloaded to>/smoke_example_case/`)

This file lists each of the sectors that assumed to be processed previously and successfully. The column "mrgapproach" indicates the temporal approach used for each sector, i.e. nonroad uses the mwdss approach including holidays (mwdss_Y) while np_solvents uses aveday without holidays (aveday_N). The "mergesector" column indicates if the sector should be merged. For the training, the sectors to be merged are airports, beis4, nonroad, np_solvents, onroad, and rwc. The point source sectors pt_oilgas, ptegu, and ptnonipm use inline emission files that will be provided as-is to CMAQ.

The merging run script will read the sectorlist file and determine which input files to provide to the SMOKE program Mrggrid for each day of the modeling period. The mapping of representative days for each day of the modeling period (August 2018) is specified in the file `$RUNSCRIPTS/smk_dates/2018/smk_merge_dates_201808.txt`

```
> cat $RUNSCRIPTS/smk_dates/2018/smk_merge_dates_201808.txt
```

```
    Date, aveday_N, aveday_Y,  mwdss_N,  mwdss_Y,   week_N,   week_Y,      all
20180801, 20180807, 20180807, 20180807, 20180807, 20180808, 20180808, 20180801
20180802, 20180807, 20180807, 20180807, 20180807, 20180809, 20180809, 20180802
20180803, 20180807, 20180807, 20180807, 20180807, 20180810, 20180810, 20180803
20180804, 20180807, 20180807, 20180811, 20180811, 20180811, 20180811, 20180804
20180805, 20180807, 20180807, 20180812, 20180812, 20180812, 20180812, 20180805
20180806, 20180807, 20180807, 20180806, 20180806, 20180806, 20180806, 20180806
...
20180830, 20180807, 20180807, 20180807, 20180807, 20180809, 20180809, 20180830
20180831, 20180807, 20180807, 20180807, 20180807, 20180810, 20180810, 20180831
```

The mrgapproach column in the sectorlist file matches the columns in the smk_merge_dates file, which lets the run script match up the appropriate representative day for each actual day in the modeling period.

Refer to [sectorlist descriptions](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#sectorlist) for more information


Switch to the merge run script directory and run the sector merge script:

```
> cd $CASESCRIPTS/merge/
```

```
> ./Sector_merge_12US1_2018gg_18j.csh
```

Mrggrid will be run for each of the days in August, combining the results from the specified sectors. Once the run script finishes,  the merged model-ready files are located in a new subdirectory inside the `$CASE_ROOT/smoke_out/` directory:

```
> cd $CASE_ROOT/smoke_out/2018gg_18j/12LISTOS/cmaq_cb6ae7/merged_withbeis_withrwc/
```

```
> ls
```

```
emis_mole_all_20180801_12LISTOS_withbeis_withrwc_2018gg_18j.ncf
emis_mole_all_20180802_12LISTOS_withbeis_withrwc_2018gg_18j.ncf
...
emis_mole_all_20180831_12LISTOS_withbeis_withrwc_2018gg_18j.ncf
README_2018gg_18j_12LISTOS_withbeis_withrwc_premerged_sectors.txt
```

The run script automatically includes "_withbeis_withrwc" when naming the files to indicate that both the BEIS4 and RWC sectors have been merged. Depending on how you plan to run the air quality model, you may want to keep these sectors separate. To do so, edit the sectorlist file, set mergesector to N for the desired sectors, and run the merge run script.
