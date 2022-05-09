{
    "title": "sndindfileerr",
    "linkTitle": "sndindfileerr",
    "weight": "3230"
}<span id="sndindfileerr"></span>

### sndindfileerr

#### CFTPARM

**[SNDINDFILEERR= <span class="underline">CONTINUE</span> &#124; ABORT ]**

Use this parameter to define the policy for a group-of-files of transfer to address an overflow issue if there is an error.

Parameter values:

- CONTINUE (default): Keep the existing behavior, which creates as many transfer requests as there are lines in the input file.
- ABORT: If the input file line is not a file, this gives the current transfer the status K diagi 132 diagp SNDINDFI, the generic transfer status is K diagi 200, and no other child requests are created.

See [Sending a group of files](../../../../concepts/send_command/send_group_of_files_cl)

[Return to Command index](../../)
