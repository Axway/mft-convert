{
    "title": "Running Transfer CFT",
    "linkTitle": "Running Transfer CFT",
    "weight": "280"
}To start the Transfer CFT server from the ****Operations**** menu, enter ********1.** Common Transfer CFT commands******, then******** **3. Start Transfer CFT**. Press ENTER to execute. Note that this starts the Transfer CFT subsystem if it was not already running.

Operations that recreate files prior to the start-up are only required in specific restart conditions, such as after changing the configuration.

> **Note**
>
> Note: If you recreate the COM file, any transfer requests deposited since Transfer CFT was last run are lost. If you recreate a CAT file, information required to restart any interrupted transfers may be lost.

Transfer CFT jobs are submitted in the following order:

- 1: CFTMAIN (main task and transfer scheduling task)

<!-- -->

- 2: CFTLOG (log management task)

<!-- -->

- 3: CFTTCOM (command management task)

<!-- -->

- 4: CFTTPRO (protocol management task)

<!-- -->

- 5: Network handler(s)

<!-- -->

- 6: CFTTFIL (file tasks)

Transfer CFT cannot run if any of the jobs are missing, except for the file task. The file task is only submitted after a send or receive transfer request, or a receive request from a remote site.
