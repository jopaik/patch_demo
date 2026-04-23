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
html_content = f"""

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
┌───────────┐    ┌─────────────┐
│ RESTORE   │    │   TASK 4:   │
│ SNAPSHOT  │───▶│    REBOOT   │
└───────────┘    └─────┬───────┘
│
▼
┌──────────────────────────┐
│ TASK 5: Compliance Update│
└──────────────────────────┘

<h2>Logic Table</h2>
<table>
    <tr><th>Task</th><th>Action</th><th>Description</th></tr>
    <tr><td>Task 1</td><td>Backup Snapshot</td><td>Mandatory baseline for PCI-DSS disaster recovery.</td></tr>
    <tr><td>Task 2</td><td>Pre_Patch_Check</td><td>Validates if patching is required.</td></tr>
    <tr><td>Task 3a/b</td><td>OS Patching</td><td>Platform-specific installation logic.</td></tr>
    <tr><td>Task 3c</td><td>Verify Patch</td><td>Checks exit codes to determine success or failure.</td></tr>
    <tr><td>Rollback</td><td>OS Rollback</td><td>Native undo (DNF Undo / WUSA Uninstall).</td></tr>
    <tr><td>Restore</td><td>Restore Snapshot</td><td>Final recovery via Hypervisor API.</td></tr>
</table>

<div class="note">
    <strong>Compliance Alert:</strong> This workflow ensures "Availability" while pursuing "Confidentiality" via patching.
</div>
with open("Patch_Workflow_Summary.html", "w") as f:
f.write(html_content)

HTML(filename="Patch_Workflow_Summary.html").write_pdf("Patch_Workflow_Report.pdf")

```python?code_reference&code_event_index=3
# Defining the markdown content
markdown_content = """# Patch_Workflow Orchestration Map

This document outlines the cross-platform automation workflow for RHEL and Windows patching, ensuring high availability and PCI-DSS compliance through multi-layer recovery logic.

## Workflow Visualization