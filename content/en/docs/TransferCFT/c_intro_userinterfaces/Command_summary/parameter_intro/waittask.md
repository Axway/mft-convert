---

    title: waittask
    linkTitle: waittask
    weight: 3750

---
<span id="waittask"></span>

### waittask

<span id="waittask_CFTPARM"></span>

#### CFTPARM

****\[WAITTASK = { <u>10</u> | n }\]   <span style="font-weight: normal;"> {1...1441}</span>****

The inactivity time, in minutes, for a file access task before it is terminated:

- <span style="font-weight: bold;">****1****</span>: Minimum value
- <span style="font-weight: bold;">****10****</span>: Waits for 10 minutes after no activity is detected before terminating the task
- **1441**: Makes the task permanent

<span style="font-weight: bold;">****z/OS:**** </span>The recommended value is **waittask=5**. This value ensures that existing tasks are reused when possible, and can reduce CPU consumption under heavy transfer loads. When the value is set to 1, unused tasks are eliminated as quickly as possible.

<span id="waittask_CFTEXIT"></span>

#### CFTEXIT

**\[WAITTASK = {<u>1441</u> | n}\]   **{1..1441}

Inactivity time, in minutes, for the EXIT task. Beyond this value, this
task is deactivated, meaning it is unloaded from the memory.

- The task is permanent if this value is equal to 1441.
- This parameter only applies to file EXIT tasks.

 

[Return to Command index](../../)
