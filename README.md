# Patch_Demo

This project demonstrates the **RHEL** and **Windows** patching process for specific **CVE** (Common Vulnerabilities and Exposures) and **KB** (Knowledge Base) IDs.

### Demo Workflow
The demo covers the following four phases:

1.  **Pre-Check**: Validating the current system state and requirements.
2.  **Patch**: Executing the update for the specified vulnerabilities.
3.  **Rollback**: Demonstrating the ability to revert changes if issues arise.
4.  **Post-Check**: Verifying successful installation and system health.


### EDA Workflow activation 

Rulebook will launch Patch_Workflow within 5secs of actions
Time of 5 secs will be used here as the event trigger

### Patch_Workflow Diagram


<h2>Workflow Visualization</h2>
<pre>
   [ START: Workflow Launched ]
              │
              ▼
   ┌──────────────────────────┐
   │ TASK 1: Backup Snapshot  │
   └──────────┬───────────────┘
              │
              ▼
   ┌──────────────────────────┐
   │ TASK 2: Pre_Patch_Check  │
   └──────────┬───────────────┘
              │
    ┌─────────┴─────────┐
    ▼                   ▼
[ IF RHEL ]         [ IF WINDOWS ]
│                   │
┌─────┴─────┐       ┌─────┴─────┐
│ TASK 3a:  │       │ TASK 3b:  │
│ RHEL Patch│       │ Win Patch │
└─────┬─────┘       └─────┬─────┘
│                   │
└─────────┬─────────┘
│
▼
┌──────────────────────────┐
│ TASK 3c: Verify Patch    │
└──────────┬───────────────┘
│
┌─────────┴─────────┐
│                   │
[ IF FAILED ]       [ IF SUCCESS ]
│                   │
▼                   │
┌───────────┐             │
│OS ROLLBACK│             │
└─────┬─────┘             │
│                   │
┌─────┴─────┐             │
│  SUCCESS? ├───────┐     │
└─────┬─────┘       │     │
│             │     │
[ IF NO ]      [ IF YES ]│
│             │     │
▼             ▼     ▼
┌───────────┐    ┌─────────────────────────┐
│ RESTORE   │    │   TASK 4:               │
│ SNAPSHOT  │───▶│    REBOOT: if required  │
└───────────┘    └─────┬───────────────────┘
│
▼
┌──────────────────────────┐
│ TASK 5: Compliance Update│
└──────────────────────────┘

## Workflow Visualization