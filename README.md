# SMOKE-ExampleCase-v2
SMOKE ExampleCase-v2 Using EPA's Emission Modeling Platform format

This SMOKE Example Case uses data from EPA's 2018v2 emissions modeling platform (https://www.epa.gov/air-emissions-modeling/2018v2-emissions-modeling-platform)

In stead of using the 12US1 (12 km grid spacing) that cover entier Continental United State and part of Canada and Mexico, this Example Case was developed for a smaller domain in 12 km grid spacing with 25x25 grid cells covering Long Island, New York and parts of the surrounding states. The specific grid comes from the Long Island Sound Tropospheric Ozone Study (LISTOS) (https://www-f.nescaum.org/documents/listos)

In this example case, the CMAQ inputs produced per day are:
+ One file of gridded, low-level emissions data
+ Multiple files containing inline point source emissions data

Entire dataset including SMOKE run scripts for this Example Case could be downloaded from [Google Drive](https://drive.google.com/file/d/1PsxZAhui7sbSt3Qx_mN1I41q936JfiwT/view?usp=sharing)

The files in this repository serve as a suite of tests for the SMOKE programs and rely on having successfully installed SMOKE and its prerequisites in your system.

List of sectors covered in this Example Case:

(i) nonpoint source sectors
  - afdust
  - rail
  - rwc
  - fertilizer
  - livestock
  - nonpt
  - nonroad
  - np_oilgas  

(ii) point source sectors
  - pt_oilgas
  - ptnonipm
  - ptfire-wild
  - ptfire-rx
  - ptegu
  - cmv_c3_12
  - cmv_c1c2_12
  - airports

(iii) On-network Mobile source (onroad)

(iv) Biogenics (BEIS4)

To run this Example Case and its components, users have to follow the next steps:

1. Download Example Case data set from the Google Drive link above.   
```
cd someplaceinyoursystem/
mkdir /path_to_your_SMOKE_example_folder/
cd /path_to_your_SMOKE_example_folder/
```
and get a copy of the SMOKE-TestCase scripts
```
git clone -b main   https://github.com/CEMPD/SMOKE-ExampleCase-v2 SMOKE-ExampleCase
```
2. Download the Test Case input data from here:  
https://drive.google.com/file/d/11ovMavOSf2ffxNukc_KJs5DfTy2tCdqQ/view?usp=share_link
*Note that in this directory, both input and output data for QA purposes are provided

Unzip the input data archive:
```
cd ..
tar -xvf smoke_training_october_2024.tar.gz
```
Unzip the output dataset needed for QA steps:
```
tar -xvf SMOKE-TestCase-output.tar.gz
```

3. Navigate to the SMOKE-TestCase scripts folder and edit run_settings.txt  
```
cd SMOKE-TestCase/scripts/
vim directory_definitions.csh
```
Modify the following paths to point to the correct for your system:

INSTALL_DIR    : Root directory (i.e. someplaceinyoursystem/ under which your keep your SMOKE-TestCase)  
MET_ROOT       : Full path of MCIP (meteorology) files (in the tarball with inputs above, MCIP output for July 2017 12US1 domain is not included, so for our TestCase downoald 12US1 inputs and link to someplaceinyoursystem/mcip) <br>

Update: 12US1 July 2017 MCIP data is available on the CMAS Data Warehouse on AWS: https://registry.opendata.aws/cmas-data-warehouse/

Browse SMOKE Test Case view readme.html for download instructions: <br>
https://cmas-smoke-testcase.s3.amazonaws.com/index.html 

MET_ROOT_3D    : Path to the full layered METCRO3D files (same as above, if you are using the TestCase inputs)  
SMOKE_LOCATION : Path to the directory with the SMOKE executables  
IOAPI_LOCATION : Path to the directory I/O API utilities  

4. Go to the scripts directory for the nonpoint sector and run the onetime script:
```
cd nonpoint
./Annual_np_oilgas_12US1_2017gb_17j_TestCase.csh >& np_oilgas_TestCase.log
```
You should be able to check the log file and and verify that the script and programs finished running successfully.
The structure within /path_to_your_SMOKE_test_folder/SMOKE-TestCase/ should have expanded and you can see your output files for this sector.  
At this point, users may want to verify the output against the relevant dataset downloaded for QA purposes in step 2.

5. Go to the scripts directory for the point sector and run the onetime script:
```
cd ../point
./Annual_pt_oilgas_onetime_12US1_2017gb_17j_TestCase.csh >& pt_oilgas_onetime_TestCase.log
```
Check the logs and verify the script and programs finished running successfully for the onetime script.
Once the onetime step is complete, run the daily script: 
```
./Annual_pt_oilgas_daily_12US1_2017gb_17j_TestCase.csh >& pt_oilgas_daily_TestCase.log
```
Verify the script and programs finished running successfully for the daily script.

## Biogenic sector

In addition, users can now test the latest version of BEIS4 to derive emission estimates for the biogenic sector.

To run the biogenic sector scripts, download and unzip the biogenic input package from the same directory and navigate to the biogenics folder:

```
cd ../biogenics
./normbeis4.final.csh >& normbeis4_TestCase.log
./tmpbeis4.2020.csh >& tmpbeis4_TestCase.log
```

Users can also download the biogenic-output dataset, found in the same folder and compare against their BEIS4 output.

If there are no errors and no differences in the output files for the above sectors your SMOKE setup is working! 

## Verifying output

Users can use the IOAPI program m3diff to check for differences in the input:
https://www.cmascenter.org/ioapi/documentation/all_versions/html/M3DIFF.html
