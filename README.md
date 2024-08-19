# hydra-clas12

### Setup the [standard clas12 environment](https://github.com/jeffersonlab/clas12-env) on ifarm
```
module use /scigroup/cvmfs/hallb/clas12/sw/modulefiles
module load clas12
```
### Create your own SQLite file and tell CCDB software to use it:
``` 
cp /scigroup/cvmfs/hallb/clas12/sw/noarch/data/ccdb/ccdb_latest.sqlite /scratch/$USER/
export CCDB_CONNECTION=sqlite:////scratch/$USER/ccdb_latest.sqlite
```
### CLI examples:
```
ccdb -i
ccdb -r 18500 dump /daq/tt/ec > ./daq-tt-ec.txt
ccdb add /daq/tt/ec -v default -r - ./daq-tt-ec.txt
```
### Python:
```python
import ccdb
provider = ccdb.AlchemyProvider()
provider.connect(os.getenv('CCDB_CONNECTION'))
print(provider.get_assignment('/daq/tt/ec',18500,'default').constant_set.data_table)
```
Not sure it's ever been used, but there's a [create_assignment](https://github.com/JeffersonLab/ccdb/blob/c30128db4c4e7799b35bf19f04ce6cf81f97f76e/python/ccdb/provider.py#L1219
) that presumably works similar to [get_assignment](https://github.com/JeffersonLab/ccdb/blob/c30128db4c4e7799b35bf19f04ce6cf81f97f76e/python/ccdb/provider.py#L1029).

