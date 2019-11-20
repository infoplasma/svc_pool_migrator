
## POOL MIGRATION COMMAND GENERATOR

#### 0. _Installation_
Download then extract the zip in to a folder of your choice (password protected, about 25 MB of space required)
***
#### 1. _Get the pool list_
This step is only needed if the pools have been modified. You can skip to step 2 otherwise.
Launch a putty session against the SVC where the source pool is located, and from there launch the following command:


```bash
lsmdiskgrp -delim ,
```
it should look like this:

```CSV
id,name,status,mdisk_count,vdisk_count,capacity,extent_size,free_capacity,virtual_capacity,used_capacity,real_capacity,overallocation,warning,easy_tier,easy_tier_status,compression_active,compression_virtual_capacity,compression_compressed_capacity,compression_uncompressed_capacity,parent_mdisk_grp_id,parent_mdisk_grp_name,child_mdisk_grp_count,child_mdisk_grp_capacity,type,encrypt,owner_type,site_id,site_name
0,V7K_N1B8B_BT11_Quorum,online,1,0,512.00MB,256,512.00MB,0.00MB,0.00MB,0.00MB,0,80,off,inactive,no,0.00MB,0.00MB,0.00MB,0,V7K_N1B8B_BT11_Quorum,0,0.00MB,parent,no,none,,
1,HUSD_53719_FIS1_NL_SAS_MIDRANGE,online,44,3,78.33TB,1024,33.10TB,92.07TB,43.39TB,45.24TB,117,80,on,balanced,no,0.00MB,0.00MB,0.00MB,1,HUSD_53719_FIS1_NL_SAS_MIDRANGE,0,0.00MB,parent,no,none,,
...
17,VIC2_HIGH_G1000_58517,online,59,2279,735.84TB,2048,373.00TB,512.81TB,353.81TB,359.75TB,69,80,on,active,no,0.00MB,0.00MB,0.00MB,17,VIC2_HIGH_G1000_58517,0,0.00MB,parent,no,none,,
```

copy the output and save it in to a a csv file with the following naming:

```
SVCPE0<x>00_DUS_pools.csv
```

***
#### 2. _Get the volume list from the pool you want to migrate_


Launch a putty session against the SVC where the source pool is located, and from there launch the following command:

```
lsvdiskcopy -delim , | head -n 1; lsvdiskcopy -delim , | grep <SOURCE_POOL_NAME>
```


copy the output and save it in to a txt file named

```
SVCPE0<x>00_DUS_<SOURCE_POOL_NAME>.csv
```

_**example**_: create the input file using lsvdiskcopy filtering for HUSA_52011_FIS1_SAS_MIDRANGE:

```
lsvdiskcopy -delim , | head -n 1; lsvdiskcopy -delim , | grep HUSA_52011_FIS1_SAS_MIDRANGE
```
Make sure to only get the required text data (include the header):
```
vdisk_id,vdisk_name,copy_id,status,sync,primary,mdisk_grp_id,mdisk_grp_name,capacity,type,se_copy,easy_tier,easy_tier_status,compressed_copy,parent_mdisk_grp_id,parent_mdisk_grp_name,encrypt
0,SANPE00005_2DC_DISK001,0,online,yes,yes,7,HUSA_52011_FIS1_SAS_MIDRANGE,10.00GB,striped,yes,on,balanced,no,7,HUSA_52011_FIS1_SAS_MIDRANGE,no
1,VDI_TE0-C2-SC_2DC_DISK001,0,online,yes,no,7,HUSA_52011_FIS1_SAS_MIDRANGE,2.00TB,striped,yes,on,balanced,no,7,HUSA_52011_FIS1_SAS_MIDRANGE,no
```
save it as:
```
SVCPE0200_DUS_HUSA_52011_FIS1_SAS_MIDRANGE.csv
```


***
#### 3. _Launch the script:_

Once you have the files ready, copy them in the same directory as the executable:

```
form_pool_migrator.exe
```
From there, the steps are straightforward, launch the executable, and select the options using 
```
<TAB>        move back and forth
<SPACE>      select item
<ARROW-KEYS> finer move
<ENTER>      confirm selection
<ESC> 
```
If the files where created correctly, you should get the output file with all commands ready. 
The output file name looks like this:

```
MIG_PE0200_2DC_HUSA_52011_FIS1_SAS_MIDRANGE__2__FIS1_MID_G800_471065.txt
```

***



