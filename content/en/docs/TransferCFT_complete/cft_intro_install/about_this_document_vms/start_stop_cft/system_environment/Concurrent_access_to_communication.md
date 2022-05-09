{
    "title": " Concurrent access to the communication file",
    "linkTitle": "Concurrent access to the communication file",
    "weight": "270"
}If users belonging to a group other than the Transfer CFT group want to submit commands in the Transfer CFT communication file, you must enable the concurrent access control mechanism at system level. The CFT_LOCK logical name is used to implement this mechanism. For more information, see the [Transfer CFT parameter settings]() section.

CFT_LOCK
---------

If the CFT_LOCK logical name is set to SYSTEM, the concurrent access control mechanism is enabled at system level. Otherwise, the control is performed at group level. The SYSLCK privilege is required to implement the mechanism. The logical name must be defined with the /JOB attribute.

Optimize receive file write operations
--------------------------------------

When receiving files, Transfer CFT allows you to optimize the way in which they are written, by performing disk accesses in virtual block mode instead of requesting that RMS write each record. The CFT$BLOCKIO logical name is used to implement this mechanism. The increased performance levels are especially noticeable when writing files containing small-sized records.

CFT$BLOCKIO
-----------

If this logical name is not defined or its substitution begins with the letter N, Transfer CFT writes the records received via RMS. In all other cases, the records are written in virtual block mode. The logical name must be defined with the /JOB attribute.
