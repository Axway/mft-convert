---

    title: Tracking and monitoring services
    linkTitle: Tracking and monitoring services
    weight: 140

---
{{< TransferCFT/axwayvariablesComponentShortName  >}} offers transfer tracking, auditing (for configuration changes), and monitoring features that provide end-to-end transfer visibility, where additionally you can link transfers with your applications. See [Using Sentinel](../../using_sentinel).

Additionally, you may implement Edge Agent reporting, as described in [Usage tracking](../../reporting).

When using governance services, {{< TransferCFT/PrimaryCGorUM  >}} provides visibility services for {{< TransferCFT/axwayvariablesComponentLongName  >}} flows and statuses. See [About Governance services](../../governance_services_intro/governance_overview).

{{< TransferCFT/PrimaryCGorUM  >}} provides:

- Monitoring of registered Transfer CFTs status through heartbeats. Transfer CFT and its Copilot send heartbeats via the persistent mutually authenticated connection with Central Governance.
- Deployment monitoring for:
    -   Configurations: The state of the last deployment configuration on a {{< TransferCFT/axwayvariablesComponentLongName >}} instance.
    -   Policies: The state of a policy deployment on each Transfer CFT linked to the policy.
    -   Flows: The state of a flow deployment on each Transfer CFT the flow uses.
    -   Updates: The status of an applied update.

For more information on {{< TransferCFT/PrimaryCGorUM  >}} functionality and features, refer to the Central Governance {{< TransferCFT/PrimaryCGversion  >}} documentation.
