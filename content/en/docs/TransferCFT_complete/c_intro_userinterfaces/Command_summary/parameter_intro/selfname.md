{
    "title": "selfname",
    "linkTitle": "selfname",
    "weight": "3160"
}<span id="selfname"></span>

### selfname

#### CFTSEND, SEND

****[SELFNAME = *filename*]  {string512}****

Name of a file that contains a list
of files selected for sending, where all of the files must be contained in the same folder.

```
send part=newyork,idf=test,selfname=selfname.txt,fname=\#myfolder
```

> **Note**
>
> Note: When using FACTION=DELETE with SELFNAME, the FNAME must be a directory (not a mask).

[Return to Command index](../../)
