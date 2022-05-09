{
    "title": "Interpreting source members",
    "linkTitle": "Interpreting source members",
    "weight": "270"
}Select option **1. Common Transfer CFT commands,** then option **6. Interpret source member** in the Operation screen to interpret source files dynamically.

These files contain change configuration commands and submit Transfer CFT commands routines.

The send file command displays the following message if syntax parsing is successful:

```
CFTU20I CFT OS/400
CFTU20I Version 3.2.1 2015/03/12
CFTU20I (C) Copyright AXWAY 1989-2015
CFTU20I ====> Starting Session on 29/03/2013 Time is 16:03:47
CFTU20I Parameters file :CFTPARM
CFTU20I Partners file :CFTPART
CFTU20I Catalog file :CFTCAT
 
CFTU20I
CFTU00I SEND _ Correct (PART=BOUCLE,IDF=TEST1,FNAME=CFTPROD/FILE1)
CFTU20I Communication file row number used: 00000010 on 20130329 Time 1603470
1
CFTU00I SEND _ Correct (PART=BOUCLE,IDF=TEST2,FNAME=CFTPROD/FILE2)
CFTU20I Communication file row number used: 00000011 on 20130329 Time 1603470
2
CFTU00I SEND _ Correct (PART=BOUCLE,IDF=TEST3,FNAME=CFTPROD/FILE3)
CFTU20I Communication file row number used: 00000012 on 20130329 Time 1603470
3
CFTU00I RETURN _ Correct (CODE=0)
CFTU20I Number of Command(s) 3
CFTU20I Number of error(s) 0
```
