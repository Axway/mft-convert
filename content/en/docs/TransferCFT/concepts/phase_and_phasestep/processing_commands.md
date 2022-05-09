{
    "title": "Transfer phase",
    "linkTitle": "Transfer  phase",
    "weight": "170"
}This is the key and only mandatory phase, which is activated for all flows. The former state and the adapted state will both be synchronized with the former state.

During this phase, the phase step is the same as the former state:

- (H) Hold: waiting for an incoming receive to start the transfer or a start command
- (D) Disposable: scheduled to start
- (C) Current: transferring/processing
- (K) Keep: transfer failed or stopped

As soon as the transfer is successfully finished, it passes to the next phase - post processing, acknowledgement, or done.

> **Note**
>
> Note: See Processing commands: general usage for a description of the processing command parameters and values.
