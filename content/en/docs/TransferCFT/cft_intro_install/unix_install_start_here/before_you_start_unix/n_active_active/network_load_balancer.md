{
    "title": "Network load balancer",
    "linkTitle": "Network load balancer",
    "weight": "200"
}This section describes how load balancing works with {{< TransferCFT/suitevariablesTransferCFTName  >}}.

{{< TransferCFT/suitevariablesTransferCFTName  >}} only uses a load balancer for incoming traffic; outgoing traffic does not require a load balancer. Additionally, incoming traffic does not connect directly to {{< TransferCFT/suitevariablesTransferCFTName  >}} processes, it is handled by the connection dispatcher (copcod), which is a child process of Copilot (copsmng).

That said, {{< TransferCFT/suitevariablesTransferCFTName  >}} actually does its own node management (through copnman, a child process of copsmng). Therefore, the Copilot socket (1766, by default) is critical for node management.

For example, you have two servers hostA and hostB each having 2 nodes:

- If the 2 nodes go down on hostA, they are restarted on hostB.
- At that point, you have 4 nodes on hostB and 0 on hostA.
    -   If Copilot is up on hostA, hostA can still receive incoming traffic, which it redirects to hostB.
    -   If Copilot is down on hostA, then that server cannot be used, and all incoming traffic is directed to hostB.
