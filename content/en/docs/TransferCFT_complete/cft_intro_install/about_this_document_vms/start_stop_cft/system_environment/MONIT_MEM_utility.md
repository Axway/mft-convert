{
    "title": "MONIT_MEM utility",
    "linkTitle": "MONIT_MEM utility",
    "weight": "240"
}The MONIT_MEM utility is used to track the Transfer CFT memory usage during processing. It displays the amount of memory that can still be allocated by the JOB, and the number and size of memory buffers shared between the tasks. It is used to fine-tune CFT$MEMCSTE, CFT$CATA, CFT$COOL, and CFT$MG_NB.

Enter the command:

```
$run cft_exe:monit_mem
```

**CFT GLOBAL MEMORY: 60/60**

```
QUEUE 0 (00064 bytes) =  35
QUEUE 1 (00128 bytes) =  11
QUEUE 2 (00256 bytes) =  10
QUEUE 3 (00512 bytes) =   9
QUEUE 4 (01024 bytes) =  10
QUEUE 5 (02048 bytes) =   9
QUEUE 6 (04096 bytes) =  10
QUEUE 7 (08192 bytes) =  10
  QUEUE 8 (16384 bytes) =  50
 
Available space in Bytes: 981568
 
CFT LOCAL MEMORY :
 
Available space in Bytes: 94192567
 
Init. threshold in Bytes: 23854496
Alarm threshold in Bytes: 23854496
Ready threshold in Bytes: 33397949
```
