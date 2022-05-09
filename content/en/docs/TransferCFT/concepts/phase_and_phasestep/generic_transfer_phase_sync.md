{
    "title": "Generic transfer phase progression",
    "linkTitle": "Generic transfer phase progression",
    "weight": "210"
}A generic transfer cannot progress to the next phase until the last of its children has passed to the following phase.

The table below shows a generic transfer A0000001, in the phase ****A**** along with its children A0000002 through A000000n.


| A<br/> Pre-processing | T<br/> Transfer | Y<br/> Post-processing | Z<br/> Ack-processing | X<br/> Done |
| --- | --- | --- | --- | --- |
| **A0000001**  |   |   |   |   |
| A0000002  |   |   |   |   |
| A0000003  |   |   |   |   |
| A0000004  |   |   |   |   |
| A0000005  |   |   |   |   |
| A000000n  |   |   |   |   |


The child transfers begin to pass through the preprocessing, transfer, and post-processing phases.


| A<br/> Pre-processing | T<br/> Transfer | Y<br/> Post-processing | Z<br/> Ack-processing | X<br/> Done |
| --- | --- | --- | --- | --- |
| **A0000001**  |   |   |   |   |
| A0000002  |   |   |   |   |
|   |   | A0000003  |   |   |
|   | A0000004  |   |   |   |
|   |   |   | A0000005  |   |
|   |   |   |   | **A000000n**  |


The generic transfer, however, must wait for its last child transfer to move to the next phase and then immediately follows its phases.


| A<br/> Pre-processing | T<br/> Transfer | Y<br/> Post-processing | Z<br/> Ack-processing | X<br/> Done |
| --- | --- | --- | --- | --- |
|   | **A0000001**  |   |   |   |
|   | A0000002  |   |   |   |
|   |   |   |   | A0000003  |
|   |   |   |   | A0000004 |
|   |   |   |   | A0000005  |
|   |   |   |   | **A000000n**  |


**Results**

The child transfers A0000003 through n are completed, and A0000002 now is in the transfer phase (the execution closely follows the last child's).

The following rules apply to the above generic/child phase processing:

- If a generic transfer is waiting for a child, its phasestep is Hold if no child in the current phase is in error, otherwise it is Keep.
- If a generic transfer needs to run a preprocessing script, it does this prior to generating its children.
- If a generic transfer needs to run a post-processing script, it waits for every child to finish their script first.
- If a generic transfer needs to run a acknowledgement script, it waits for every child to finish their script first.

You can set the policies defining which transfer executes the script using the execution parameters described in [Processing execution policy](../../about_transfer_processing/processing_exec_policy).
