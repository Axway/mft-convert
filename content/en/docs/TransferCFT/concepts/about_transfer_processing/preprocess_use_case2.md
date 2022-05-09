{
    "title": "Use case: replace variable and zip file",
    "linkTitle": "Usage: zip a file before sending",
    "weight": "200"
}**Example in UNIX**

This use case demonstrates how to replace a variable in a file, and then zip the file prior to processing the transfer.

1. In a command line window, enter: `cd $CFTDIRRUNTIME`
1. Create your sample file: `echo “Hello _user_” > Hello`

    In this example the ****Hello**** file is in $CFTDIRRUNTIME.  

1. Create and display your sample script `cat myexec.sh:`
    ```
    ! /bin/sh
    if [ "&APPSTATE" = "" ]; then
    CFTUTIL end part=&PART,idtu=&IDTU,istate=yes,appstate="0%"
    fi
    if [ "&APPSTATE" = "0%" -o "&APPSTATE" = "" ]; then
    tmpfile=&FNAME_&PART
    sed s/_user_/&PART/g <&FNAME >$tmpfile
    CFTUTIL end part=&PART,idtu=&IDTU,istate=yes,appstate="50%",fname=$tmpfile
    else
    tmpfile=&FNAME
    fi
    if [ "&APPSTATE" = "50%" -o "&APPSTATE" = "0%" -o "&APPSTATE" = "" ]; then
    outputfile=$tmpfile.tgz
    tar zcvf $outputfile $tmpfile
    CFTUTIL end part=&PART,idtu=&IDTU,istate=no,appstate="100%",fname=$outputfile
    rm $tmpfile
    fi
    ```
1. Execute the following command, where `preexec `points to your script:

    `CFTUTIL send part=paris,idf=test,fname=$CFTDIRRUNTIME/Hello,preexec=$CFTDIRRUNTIME/myexec.sh`

****Results****

The script replaces ` _user_` in the ****Hello**** file with the PARTNER name, and then compress this modified file. At the end of the preprocessing , the tar file is sent to the partner. The partner can set an exec for this idf, `test `in our example, that will untar the received file.

> **Note**
>
> Note: Notification is done by the CFTUTIL end istate=no, where no is the default istate value.
