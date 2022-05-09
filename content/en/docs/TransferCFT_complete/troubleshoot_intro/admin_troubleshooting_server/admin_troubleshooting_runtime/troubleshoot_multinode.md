{
    "title": "Troubleshoot multi-node",
    "linkTitle": "Troubleshoot multi-node",
    "weight": "450"
}Multi-node starts but is not functional
---------------------------------------

****Symptom****

In a multi-node configuration, if a host address is incorrectly set in uconf, COPSMNG and all Transfer CFT tasks start correctly. However, CFTPRX cannot establish connection with the other nodes because it is set to the incorrect address.

****How to diagnose****

Once all nodes are started, check the log for this error message "CFTPRX error _ Failed to connect to NODE0".

****How to fix****

Reconfigure the host address as follows:

1. Stop the Transfer CFT instance.
1. Identify the host with the incorrect host address.
1. Remove the host using the command:
    ```
    cft remove_host -hostname <hostname>
    ```
1. Add the host with the correct host address:  
    ```
    cft add_host -hostname <hostname> -host <host_address>
    ```
