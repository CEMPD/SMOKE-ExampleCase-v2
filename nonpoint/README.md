**Overall Instructions**

Similarly to structure of EPA Emission Modeling Platform (EMP), in this SMOKE Example Case this directory contains run scripts 
for nonpoint sectors including `afdust`, `rail`, `np_oilgas`, `np_solvents`, `rwc`, `nonpt`, `livestock`, `fertilizer`, `nonroad`. 
Refer to [Run Script Structure for nonpoint](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#casescriptsnonpoint) for more information

Before running SMOKE processing script for any source sector, it is recommended to activate all environmental valriable settings in `directory_definitions.csh` and use them as short-cuts to navigate to various I/O directories

```
source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

Refer to [Run Script Variables](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#run-script-variables) for descriptions of major variables in the run scripts.


**Example: Processing a Nonpoint Sector (Residential Wood Combustion)**

In this example, you'll run SMOKE for the nonpoint sector "rwc". This sector consists of residential wood combustion emissions, which are emissions from home wood burning devices like fireplaces, wood stoves, and outdoor fire pits.

To get started, source the directory definitions script:

```
source <directory where Example Case data was downloaded to>/smoke_example_case/2018gg_18j/scripts/directory_definitions.csh
```

Now switch into the directory containing the nonpoint sector run scripts:

```
cd $CASESCRIPTS/nonpoint
```

Run the ls command to see a list of scripts in this directory:

```
ls
```

The above command should list the following:

```
Annual_afdust_12US1_2018gg_18j.csh
Annual_afdust_adj_12US1_2018gg_18j.csh
Annual_np_oilgas_12US1_2018gg_18j.csh
Annual_np_solvents_12US1_2018gg_18j.csh
Annual_rail_12US1_2018gg_18j.csh
Annual_rwc_12US1_2018gg_18j.csh
Monthly_fertilizer_12US1_2018gg_18j.csh
Monthly_livestock_12US1_2018gg_18j.csh
Monthly_nonpt_12US1_2018gg_18j.csh
Monthly_nonroad_12US1_2018gg_18j.csh
```

Each nonpoint sector has a separate run script. To run the RWC sector script:

```
./Annual_rwc_12US1_2018gg_18j.csh
```

As the script runs, you'll see many messages scroll on the screen. Some of these messages are checks that the script is doing on the input files, like checking for duplicate records in cross-reference files. You'll also see the individual SMOKE programs being run. For the RWC sector, the following SMOKE programs need to be run:

`Smkinven`: imports the inventory containing residential wood combustion emissions
`Spcmat`: applies the speciation profiles to create the speciation matrix
`Grdmat`: applies spatial surrogates to create the gridding matrix
`Smkreport`: create various reports of the emissions at different stages of processing and detail levels
`Temporal`: applies temporal profiles to calculate hourly emissions
`Smkmerge`: merges the hourly emissions with the speciation matrix and gridding matrix to create model-ready outputs

For the RWC sector, the program Smkmerge is run for every day in the episode, i.e. each day in August. The script will take a few minutes to run.

After running all the SMOKE programs, the run script analyzes the log files produced by the SMOKE programs and checks for any problems. At the end of the messages printed by the script, you'll see a summary like so:

```
Finished getting data
Classifying message types...
Total number of known messages: 468
Total number of unknown messages: 0
Level 1 analysis...
Finished classifying message types
Testing for exit priority <=  1
All message priorities >  1
The script has completed successfully!
```

**NOTE:** Although `Annual_np_oilgas_12US1_2018gg_18j.csh` is provided in the SMOKE Example Case, executing this script will eventually failed due to no nonpoint Oil and Gas emission sources in the model domain of the Example Case.
