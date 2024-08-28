# hydra-clas12
### Setup the [clas12 (dev) environment](https://github.com/jeffersonlab/clas12-env) on ifarm
```
module use /scigroup/cvmfs/hallb/clas12/sw/modulefiles
module load clas12/dev
```
Note, the "dev" is important to pickup the latest mon12.
### Create your own SQLite file, tell CCDB software to use it, and give yourself permissions to modify it
``` 
cp /scigroup/cvmfs/hallb/clas12/sw/noarch/data/ccdb/ccdb_latest.sqlite /scratch/$USER/
export CCDB_CONNECTION=sqlite:////scratch/$USER/ccdb_latest.sqlite
echo $USER | python3 $CCDB_HOME/scripts/users_create/users_create.py $CCDB_CONNECTION
```
### CCDB CLI examples
```
ccdb -i
ccdb -r 18500 dump /daq/tt/ec > ./daq-tt-ec.txt
ccdb add /daq/tt/ec -v default -r - ./daq-tt-ec.txt
```
### CCDB Python example
```python
import ccdb
provider = ccdb.AlchemyProvider()
provider.connect(os.getenv('CCDB_CONNECTION'))
provider._auth.current_user_name = os.getenv('CCDB_USER')
print(provider.get_assignment('/daq/tt/ec',18500,'default').constant_set.data_table)
```
Note, there's a [create_assignment](https://github.com/JeffersonLab/ccdb/blob/c30128db4c4e7799b35bf19f04ce6cf81f97f76e/python/ccdb/provider.py#L1219
) that presumably works similar to [get_assignment](https://github.com/JeffersonLab/ccdb/blob/c30128db4c4e7799b35bf19f04ce6cf81f97f76e/python/ccdb/provider.py#L1029), otherwise use intermediate text files.

### Mon12
```
mkdir ./mon12.tmp
mon12 -tabs ECAL -trigger 0x1 -autosave 10000 -outDir ./mon12.tmp /path/to/evio/file
```
Note, the best setting for trigger depends on the run group, but for ECAL/FTOF 0x1 is always fine.
### Input Data
```
/cache/clas12/rg-k/data/clas_019335
/cache/clas12/rg-d/data/clas_018375
```
