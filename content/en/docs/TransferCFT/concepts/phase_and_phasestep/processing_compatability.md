---
title: "Transfer CFT backward compatibility"
linkTitle: "Transfer CFT backward compatibility "
weight: 220
--- In some cases, you may require or prefer backward compatibility. In this case, the `uconf:cft.state_compat` and `uconf:cft.listcat_compat` parameters can provide the same functionality as in Transfer CFT 2.7.1.

The default value (No) sets:

- LISTCAT to version 3.0 or higher
- State to an adapted state, which corresponds to the phase and phase steps described in this section

![](/Images/TransferCFT/temp_compat.png)

| Phase  | Phasestep  | State  | Description  |
| - - - | - - - | - - - | - - - |
| Pre- processing (A)  | Hold (H)  | Pre- processing (A)  | Pre- processing available  |
| Pre- processing (A)  | Available (D)  | Pre- processing (A)  | Pre- processing is waiting on a resource to start  |
| Pre- processing (A)  | Processing (C)  | Pre- processing (A)  | Pre- processing in progress  |
| Pre- processing (A)  | Killed (K)  | Pre- processing (A)  | Pre- processing is canceled  |
| Transfer (T)  | Hold (H)  | Hold (H)  | Transfer available (diagi=0) or interrupted (diagi &lt;&gt; 0)  |
| Transfer (T)  | Available (D)  | Available (D)  | Transfer is waiting resource to start  |
| Transfer (T)  | Processing (C)  | Processing (C)  | Transfer is in progress  |
| Transfer (T)  | Killed (K)  | Killed (K)  | Transfer is canceled  |
| Post- processing (Y)  | Hold (H)  | Transfer finished (T) or post- processing executed (X)  | Post- processing is interrupted  |
| Post- processing (Y)  | Exit EOT (E)  | Transfer finished (T) or post- processing executed (X)  | Exit in progress  |
| Post- processing (Y)  | Available (D)  | Transfer finished (T) or post- processing executed (X)  | Post- processing is waiting resource to start  |
| Post- processing (Y)  | Processing (C)  | Transfer finished (T) or post- processing executed (X)  | Post- processing in progress  |
| Post- processing (Y)  | Killed (K)  | Transfer finished (T) or post- processing executed (X)  | Post- processing is canceled  |
| Ack (Z)  | Hold (H)  | Transfer finished (T) or post- processing executed (X)  | Ack processing is interrupted  |
| Ack (Z)  | Processing (C)  | Transfer finished (T) or post- processing executed (X)  | Ack processing is not started  |
| Ack (Z)  | Available (D)  | Transfer finished (T) or post- processing executed (X)  | Ack processing in progress  |
| Ack (Z)  | Killed (K)  | Transfer finished (T) or post- processing executed (X)  | Ack processing is canceled  |
| Done (X)  | Done (X)  | Done (T or X)  | Flow finished  |

<span id="Compatibility unified configuration parameters"></span>

## Compatibility parameters in unified configuration

| Parameter  | Default value  | Description  |
| - - - | - - - | - - - |
| Uconf:cft.listcat_compat  | No  | Defines the LISTCAT display:<br/> • Yes = Display using the former product format, which does not include the new columns. The format in LISTCAT is DTSA.<br/> • No= Display using the product version 3.0 and higher catalog format. The format in LISTCAT is DTSASPP. |
| Uconf:cft.state_compat  | No  | Defines the transfer states:<br/> • Yes= The phase state is fully compatible with the states in versions prior to 3.0.<br/> • No = The state reflects the phase used in Transfer CFT 3.0 and higher. This uses phase instead of the former states, except during the Transfer phase, when the former state is the same as the phase step.<br/> ****Note****: Uconf:cft.state_compat also impacts the [acknowledgement](../ack_phase) behavior if ackstate is set to ignore. |

