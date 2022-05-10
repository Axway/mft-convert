---
    "title": "Access management  reference information ",
    "linkTitle": "Reference: access management mapping",
    "weight": "210"
---
{{% TransferCFT/snippets/access_management%}}

Access management mapping in earlier versions
---------------------------------------------

Prior to the PassPort AM implementation and interoperability, Transfer
CFT used the following Access Management setup. This information is provided as a reference.

### Access Management without PassPort AM

```
        Appl
(ID)
            (control:
end,halt,keep,start,submit)
            (create:
send,recv)
            (delete:
delete_catalog)
            (read:
view_catalog)
            (modify:
resume)
    Command:
          Shut
             create
          Switch_log,
             create
          Switch_accnt,
             create
          Act,
             create
          Inact,
              create
          Mquery,
              create
          Turn,
              create
    File
          File
(FNAME)
               (read)
               (delete)
    Message:
           Message
(IDM,PART,SPART,RPART,MODE)
                create
    Operator:
            ALL_PART
(fname)
                    create,delete,read,modify
            ALL_CAT
(fname)
                    control(see
appl), delete, read, modify (resume)
            ALL_COM
(fname)
                    create,
read, delete
    Parameter:
             CFT
obj (ID)
                    create,delete,read,modify
    Partner:
             CFT
obj (ID)
                    create,delete,read,modify
     
    Transfer:
             Transfer
(IDF,PART,SPART,RPART,IPART,MODE,FNAME)
                    create
             Commut
(PART,IPART)
                    create
```

About resources and actions
---------------------------

You can map the environment variables as displayed
in the following examples.

### Configuration

`    PKI....     (ID)   Create/Delete/View/Edit (New)`

`    CFT....     (ID)   Create/Delete/View/Edit`

`    CFTPART     (ID)   Create/Delete/View/Edit/Turn/Activate/Deactivate `

`    CFTCRON     (ID)   Create/Delete/View/Edit/Activate/Deactivate/Reload `

`    CFTUCONF    (ID) Create/Delete/View/Edit   (New)`

###  Service

`        CATALOG          Purge      `

`        LOG                   Switch   `

`        ACCOUNT         Switch   `

`        SENTINEL        Activate/Inactivate   `

`        BATCH(ID,FNAME)   Execute/View/Edit `

`        CFTNAV            Startup/Shutdown/Restart/Trace   `

`        CFTSRV             Startup/Shutdown/Restart/Trace`

`        UI(TYPE=CFTUTIL/CFTNAVIGATOR/IUI/WEBSERVICE)   Connect                             `

`        PROBE(ID)        View`

`        CACHE   `

`             CATALOG       View`

`             CRON             View`

`             COMMAND    View `

`             DMZ               View   `

`         COM                  Delete/View      `

### Transfer

`(IDAPPL, ID, PART, SPART , RPART, IPART, TYPE, DIRECT, MODE, FNAME, MESSAGE, USERID)   `

`          Create     SEND/RECV`

`          Delete     DELETE`

`          View        LISTCAT   `

`          Edit         KSTATE`

`          Cancel     KEEP`

`          Resume    RESUME/START`

`          Pause       HALT`

`          Execute      `

`          Submit   `

`          "End"`

`          ViewFile   `

`          EditFile   `

`          DeleteFile`

###    Filter

`  CATALOG(ID)   Create/Edit/View/Delete`

`  LOG(ID)            Create/Edit/View/Delete   `

`   FILE(FNAME)          Create/Edit/View/Delete`

`   URL(URL)                 View          `

`   [   FILE                (FNAME)    Create*/View/Delete   ]`

`   [   VFMFILE         (FDB,   FNAME) Create/Delete/View/Edit ] `
