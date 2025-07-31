# SMOKE-ExampleCase-v2

This SMOKE Example Case uses data from EPA's 2018v2 emissions modeling platform (https://www.epa.gov/air-emissions-modeling/2018v2-emissions-modeling-platform)

In stead of using the 12US1 (12 km grid spacing) that covers entire the Continental United State and part of Canada and Mexico, this Example Case was developed for a smaller domain in 12 km grid spacing with 25x25 grid cells covering Long Island, New York and parts of the surrounding states. The specific grid comes from the Long Island Sound Tropospheric Ozone Study (LISTOS) (https://www-f.nescaum.org/documents/listos)

Modeling episode: August 1st through 31st, 2018; Chemical mechanism: CB6AE7

In this example case, the CMAQ inputs produced per day are:
+ One file of gridded, low-level emissions data
+ Multiple files containing inline point source emissions data

Entire dataset including SMOKE run scripts for this Example Case could be downloaded from [Google Drive](https://drive.google.com/file/d/1PsxZAhui7sbSt3Qx_mN1I41q936JfiwT/view?usp=sharing)

List of sectors covered in this Example Case:

*(i) nonpoint source sectors*
  - afdust
  - rail
  - rwc
  - fertilizer
  - livestock
  - nonpt
  - nonroad
  - np_oilgas  

*(ii) point source sectors*
  - pt_oilgas
  - ptnonipm
  - ptfire-wild
  - ptfire-rx
  - ptegu
  - cmv_c3_12
  - cmv_c1c2_12
  - airports

*(iii) On-network Mobile source (onroad)*

*(iv) Biogenics (BEIS4)*

The files in this repository only serve as documentations providing instructions for processing emissions of serveral example source sectors. Overall concepts of emission modeling platform and its SMOKE run script structure are subsequently introduced in these documentations assuming the source sectors are being processed in the order of nonpoint, point, road, beis4, and followed by running `Mrggrid` to create files ready for the air quality model.  

To run this Example Case and its components, users have to follow the next steps:

### 1. Download Example Case data set from the Google Drive link above ###

### 2. Unzip Example Case data: ###
```
cd <directory where Example Case data was downloaded to>/
tar -xvzf smoke_example_case.June2025.tar.gz
cd smoke_example_case
```
All Example Case data is now extracted to sub-folder `smoke_example_case` that contains all input data and SMOKE run scripts.

### 3. Examine content of Example Case ###
```
cd smoke_example_case
ls -al
```
The above comand should lists the following directories:
- `2018gg_18j`: contains case-specific inputs and run scripts for a emission scenarios 
- `ge_dat`: contains general ancillary files including those related to speciation, spatial allocation (gridding), temporalization profiles and profile cross-reference files. This data is not case-specific
- `ioapi`: contains precompiled utility programs from the I/O API library
- `met`: Meteorology input files that had been processed by the Meteorology-Chemistry Interface Processor (MCIP)
- `smoke5.1`: contains individual precompiled SMOKE executable programs (e.g. Smkinven, Grdmat, Spcmat, etc.) and various helper scripts

For more information, refer to [Directory Structure](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#directory-structure) on SMOKE Wiki page. 


### 4. Customize SMOKE Example Case setup for new system ###
```
cd smoke_example_case/2018gg_18j/scripts/
vim directory_definitions.csh
```
Modify the setting of `INSTALL_DIR` to point to the correct path for your system under which all data from SMOKE Example Case was extracted to (i.e. `<directory where Example Case data was downloaded to>/smoke_example_case`)  

Settings for other paths could remain unchanged. Refer to [Customizing EMP setups for new system](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#customizing-emp-setups-for-new-system) for descriptions of other settings in `directory_definitions.csh` script

### 5. Update SMOKE executables (if needed) ###

The precompiled SMOKE executable programs (e.g. Smkinven, Grdmat, Spcmat, etc.) included in this Example Case were compiled from SMOKE v5.1 with latest updates as of June 2025. In situation where the precompiled SMOKE executable programs fail to be executed on local machine, consult [Instruction for SMOKE Installation](https://github.com/CEMPD/SMOKE/wiki/B.-Instructions-for-SMOKE-Installation) to obtain the latest SMOKE source code and compile SMOKE programs on local machine. Refer to [Customizing EMP setups for new system](https://github.com/CEMPD/SMOKE/wiki/A.-Overall-Instructions-on-Running-SMOKE-using-EPA's-Emissions-Modeling-Platforms#customizing-emp-setups-for-new-system) for instruction to specify location of the new SMOKE executable programs (i.e., `setenv SMOKE_LOCATION <path to new SMOKE executables>`) 




Up to this point, you are now ready to run SMOKE to process emissions for each of the source sectors in this Example Package. Refer to instruction in sub-sequence directories in this Github page for futher instructions.
