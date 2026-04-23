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
graph TD
    Start([START: Workflow Launched]) --> T1[TASK 1: Take Backup Snapshot]
    T1 --> T2[TASK 2: Run Pre_Patch_Check]
    
    T2 --> OS_Branch{OS Branch}
    
    OS_Branch -- RHEL --> T3a[TASK 3a: RHEL_Patch]
    OS_Branch -- Windows --> T3b[TASK 3b: Windows_Patch]
    
    T3a --> T3c[TASK 3c: Run Verification Checks]
    T3b --> T3c
    
    T3c -- SUCCESS --> T4[TASK 4: System Reboot]
    T3c -- FAILED --> RB_Branch[OS ROLLBACK - RHEL/Win]
    
    RB_Branch -- ROLLBACK SUCCESS --> T4
    RB_Branch -- ROLLBACK FAIL --> T3d[Restore Snapshot - Hypervisor]
    
    T3d --> T4
    T4 --> T5[TASK 5: Compliance Reporting Update]
    T5 --> End([END: Process Complete])

    style T3d fill:#f96,stroke:#333,stroke-width:2px
    style RB_Branch fill:#f9f,stroke:#333
    style T3c fill:#ccf,stroke:#333