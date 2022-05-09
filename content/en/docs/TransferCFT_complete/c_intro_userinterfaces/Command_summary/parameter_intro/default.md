{
    "title": "default",
    "linkTitle": "default",
    "weight": "650"
}<span id="Default"></span>

### default

#### CFTPARM

Optionally you can enter a default identifier to use in the CFTRECV,
CFTSEND, CFTXLATE, and CFTAPPL objects.

The default value is DEFAULT.

Default identifier generically indicated as &lt;default&gt; for the CFTRECV, CFTSEND, and CFTXLATE commands.

#### CFTRECV and CFTSEND

ID = &lt;default&gt; These commands specify the complementary
default values of the RECV and SEND command parameters, when the transfer
request IDF is not explicitly defined by a CFTRECV or CFTSEND command.
The commands CFTRECV and CFTSEND ID = &lt;defaut&gt; are mandatory.

Note: the characteristics of the default model files (CFTSEND and CFTRECV
with a default identifier) are, unlike the other model files, loaded into
memory on {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization. These characteristics are consequently
STATIC parameters.

The use of the CFTRECV ID = &lt;default&gt; command may involve
the reception of files of different sizes and formats. When the protocol
allows, it is consequently best for the values of the parameters FSPACE,
FRECFM, FORG (etc.) of this command not to be defined, so that Transfer
CFT takes into account the actual values of the file received, as transferred
by the protocol (see the features of each protocol).

#### CFTXLATE

ID = &lt;default&gt;  
This command specifies the default translation tables, when not explicitly
designated by the SEND/CFTSEND (or RECV/CFTRECV) and CFTPART commands.
The command CFTXLATE ID = &lt;defaut&gt; is optional; if it is
not included, the monitor internal translation tables are used (see their
description in the appendix).

[Return to Command index](../../)
