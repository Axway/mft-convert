{
    "title": "waittask",
    "linkTitle": "waittask",
    "weight": "3770"
}<span id="waittask"></span>

### waittask

<span id="waittask_CFTPARM"></span>

#### CFTPARM

****[WAITTASK = { <span class="underline">10</span> &#124; n }]    {1...1441}****

The inactivity time, in minutes, for a file access task before it is terminated:

- ****1****: Minimum value
- ****10****: Waits for 10 minutes after no activity is detected before terminating the task
- **1441**: Makes the task permanent

****z/OS:**** The recommended value is **waittask=5**. This value ensures that existing tasks are reused when possible, and can reduce CPU consumption under heavy transfer loads. When the value is set to 1, unused tasks are eliminated as quickly as possible.

<span id="waittask_CFTEXIT"></span>

#### CFTEXIT

**[WAITTASK = {<span class="underline">1441</span> &#124; n}]   **{1..1441}

Inactivity time, in minutes, for the EXIT task. Beyond this value, this
task is deactivated, meaning it is unloaded from the memory.

- The task is permanent if this value is equal to 1441.
- This parameter only applies to file EXIT tasks.

 

[Return to Command index](../../)
