## Overal Instructions

Similarly to structure of EPA Emission Modeling Platform (EMP), in this SMOKE Example Case this directory contains run scripts 
for point sectors including `ptnonipm`, `ptfire-wild`, `ptfire-rx`, `ptagfire`, `ptegu`, `pt_oilgas`, `cmv_c3_12`, `cmv_c1c2_12`, and `airports`. 
Refer to [Run Script Structure for point](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#casescriptspoint) for more information



## Example Case: Processing Airports Point Sector

In this example, you'll process the airports point source sector. This sector consists of emissions from aircraft and ground support equipment used at the airport. Compared to the previous nonpoint sectors, the inventory data contains a latitude and longitude for each source, which is used to determine which grid cell the emissions should be allocated to.

The airports inventory doesn't contain stack parameters and the sources will not be treated as elevated.

First, source the directory definitions script:


```
> source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

Now switch into the directory containing the point sector run scripts:

```
> cd $CASESCRIPTS/point
```

Run the ls command to see a list of scripts in this directory:

```
> ls *airports*
```

The above command should list the following:

```
Annual_airports_daily_12US1_2018gg_18j.csh
Annual_airports_onetime_12US1_2018gg_18j.csh
```

The onetime script always needs to be run first for any point sector, so go ahead and run the onetime script for airports:

```
> ./Annual_airports_onetime_12US1_2018gg_18j.csh
```

The script will run the SMOKE programs Smkinven, Spcmat, Grdmat, and Smkreport. When Grdmat runs, it won't use spatial surrogates but instead assigns sources to grid cells based on the source's coordinates.

Similar to the nonpoint sectors, the log analyzer will check for any errors and signal a successful finish.

```
Finished getting data
Classifying message types...
Total number of known messages: 40
Total number of unknown messages: 0
Level 1 analysis...
Finished classifying message types
Testing for exit priority <=  1
All message priorities >  1
```

Next, run the daily script for airports:

```
> ./Annual_airports_daily_12US1_2018gg_18j.csh
```

This script runs Temporal and Smkmerge. The airports sector uses "week" temporal handling so Smkmerge is run for Monday, August 6 through Sunday, August 12, 2018.

The outputs from the airports sector are located in the directory $CASE_ROOT/premerged/airports and are similar to previous nonpoint outputs since the emissions are not elevated.

```
> cd $CASE_ROOT/premerged/airports
```

```
> ls
```

```
emis_mole_airports_20180806_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_airports_20180807_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_airports_20180808_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_airports_20180809_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_airports_20180810_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_airports_20180811_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
emis_mole_airports_20180812_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
```

These ground-level emissions will be merged with the other non-elevated emissions in a later merge step.


## Example Case: Processing *ptnonipm* Point Sector

Processing steps for ptnonipm are similar to other point sectors. Make sure you're in the point sector run scripts directory and then run the ptnonipm onetime script:

```
> source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

```
> cd $CASESCRIPTS/point
```

```
> ./Annual_ptnonipm_onetime_12US1_2018gg_18j.csh
```

The script will run the SMOKE programs Smkinven, Spcmat, Grdmat, Smkreport, and Elevpoint.

The SMOKE program Elevpoint is used to specify which point sources should be treated as elevated. This process is controlled by the PELVCONFIG configuration file. Scroll through the ptnonipm onetime script to find the setting for PELVCONFIG:

```
> more Annual_ptnonipm_onetime_12US1_2018gg_18j.csh
```

```
setenv PELVCONFIG "${GE_DAT}/point/pelvconfig_elevate_everything_17apr2020_v0.txt"
```

The PELVCONFIG file is located in the $GE_DAT/point directory.

```
> cat $GE_DAT/point/pelvconfig_elevate_everything_17apr2020_v0.txt
```

```
#Copied from version 1 of dataset pelvconfig_seca_c3 on Fri Apr 17 13:13:04 EDT 2020
#EXPORT_DATE=Tue Feb 09 11:01:54 EST 2021
#EXPORT_VERSION_NAME=Initial Version
#EXPORT_VERSION_NUMBER=0

SMK_SOURCE P

/SPECIFY ELEV/
HT => 0.
/END/
```

In the PELVCONFIG file, criteria for selecting elevated sources are listed inside the section between the lines /SPECIFY ELEV/ and /END/. In this case, any source with a stack height greater than or equal to zero (HT >= 0.) will be treated as elevated. This means that all sources are elevated and thus will be included in the stack groups file output by Elevpoint and the inline emissions files output by Smkmerge.

Since the ptnonipm onetime run script ran the Elevpoint program, this means that the stack groups output file for the ptnonipm sector has been created. This file is put into a subdirectory of the $CASE_ROOT/smoke_out directory. The smoke_out directory is used for final output files, ready to be used as inputs to CMAQ. The outputs you've seen from running other sectors have been put into sector-specific directories inside $CASE_ROOT/premerged. These "premerged" outputs still need to go through a final merge step to create outputs in smoke_out. However, the stack groups file created by Elevpoint doesn't need further processing.

For now, the only output in the ptnonipm smoke_out directory is the stack groups file that Elevpoint just created:

```
> ls $CASE_ROOT/smoke_out/2018gg_18j/12LISTOS/cmaq_cb6ae7/ptnonipm/
```

```
stack_groups_ptnonipm_12LISTOS_2018gg_18j.ncf
```

The stack groups file is a list of elevated point sources (16,872 for the model domain in this Example Case) with coordinates, stack parameters, and grid cell assignments for each source.

After running the onetime programs for the ptnonipm sector, let's run the daily programs. If you've switched directories, return to the point sector run scripts directory and then run the ptnonipm daily script:

```
> cd $CASESCRIPTS/point
```

```
> ./Annual_ptnonipm_daily_12US1_2018gg_18j.csh
```

The ptnonipm daily script will run the SMOKE programs Temporal and Smkmerge. The ptnonipm sector uses the Monday-weekday-Saturday-Sunday (mwdss) temporal approach, which is the same as the nonpoint sectors. This means that Smkmerge was run for Monday, August 6, 2018, Tuesday the 7th, Saturday the 11th, and Sunday the 12th.

Unlike previous sectors, Smkmerge hasn't created any files in the premerged directory $CASE_ROOT/premerged/ptnonipm/. Instead, the emissions are output to inline emissions files in the smoke_out directory: 

```
ls $CASE_ROOT/smoke_out/2018gg_18j/12LISTOS/cmaq_cb6ae7/ptnonipm/
```

```
inln_mole_ptnonipm_20180806_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
inln_mole_ptnonipm_20180807_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
inln_mole_ptnonipm_20180811_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
inln_mole_ptnonipm_20180812_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
stack_groups_ptnonipm_12LISTOS_2018gg_18j.ncf
```

In addition to the stack groups file created by Elevpoint, now the directory contains an inline emissions file (inln_mole_ptnonipm_*) for each of the four days that Smkmerge ran. These files contain hourly speciated emissions for each source listed in the stack groups file. When used as inputs to CMAQ, the air quality model can calculate plume rise based on the data in the stack groups file and appropriately allocate the hourly emissions to each model layer.



## Example Case: Processing Oil & Gas Point Sector as layered, gridded emissions
This example case assumes that Oil & Gas point sectors had been processed for inline plume rise treatment in the photochemical model such as CMAQ (i.e., `Annual_pt_oilgas_onetime_12US1_2018gg_18j.csh` and then `Annual_pt_oilgas_daily_12US1_2018gg_18j.csh` were run).

As an alternative to processing Oil & Gas point source emissions for inline plume rise treatment, SMOKE can handle plume rise calculation using the Laypoint program to create layered, gridded emission files:

```
> cd $CASESCRIPTS/point
```

```
> ./Annual_pt_oilgas_3d_onetime_12US1_2018gg_18j.csh
```

Similar to the inline approach, the 3d_onetime script will run Smkinven, Spcmat, Grdmat, Smkreport, and Elevpoint for the oil & gas sector point source emissions.

After the onetime script is complete, you'll see that Elevpoint has created the stack groups output file in the sector-specific smoke_out subdirectory:

```
> ls $CASE_ROOT/smoke_out/2018gg_18j/12LISTOS/cmaq_cb6ae7/pt_oilgas_3d/
```

```
stack_groups_pt_oilgas_3d_12LISTOS_2018gg_18j.ncf
```

Now run the pt_oilgas_3d daily script:

```
> ./Annual_pt_oilgas_3d_daily_12US1_2018gg_18j.csh
```

This pt_oilgas_3d script calls the Laypoint program. Unlike the inline approach, output files are created under the $CASE_ROOT/premerged directory as layered, gridded emission files:

```
> ls $CASE_ROOT/premerged/pt_oilgas_3d/
```

```
emis_mole_pt_oilgas_3d_20180807_12LISTOS_cmaq_cb6ae7_2018gg_18j.ncf
```
