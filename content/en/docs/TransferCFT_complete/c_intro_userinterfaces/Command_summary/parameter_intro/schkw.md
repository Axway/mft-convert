{
    "title": "schkw",
    "linkTitle": "schkw",
    "weight": "3090"
}<span id="schkw"></span>

### schkw

#### CFTPROT

****[SCHKW =  {
3
&#124; n }]   {0...12}****

A value between ****0**** and ****12**** that represents the maximum for the
synchronization points that are not acknowledged (default = 3). This value is negotiated
with the receiver partner.

Size of the send mode acknowledgement anticipation window for synchronization
points, expressed as a number of synchronization points not acknowledged.
The value is negotiated during the connection phase with the partner RCHKW
value. This window is used to send the data before the acknowledgments
are received. SCHKW=0 means that synchronization points are not acknowledged.

 

[Return to Command index](../../)
