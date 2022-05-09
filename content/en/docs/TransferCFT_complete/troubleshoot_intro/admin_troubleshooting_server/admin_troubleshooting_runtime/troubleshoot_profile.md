{
    "title": "Troubleshoot error messages when loading the profile",
    "linkTitle": "Troubleshoot error when loading profile",
    "weight": "440"
}If error messages display when loading the profile, for example "CFTDIRDAT parameter not set", the error may be due to a new file in the $CFTDIRRUNTIME/profile.d directory. An example of this is `cft-autocomplete.bash_completion`, which was introduced in {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.3.0.

To fix this issue, you can run the following command :

```
cftruntime -p $CFTDIRINSTALL $CFTDIRRUNTIME
```

This command saves the current profile and creates a new one. You can compare the previous profile file with the new one, and keep the file you require.
