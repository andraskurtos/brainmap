# Phase 1

## System Breakdown and Initial Diagrams

In the first phase of the project, we structured the system into four major subsystems:
- **AMS (Airspace Management System)**
- **GCS (Ground Control System)**
- **PHS (Package Handling Station)**
- **UAV (Unmanned Aerial Vehicle)**

For this phase, we focused on defining the system architecture using **Block Definition Diagrams (BDD)** and **Internal Block Diagrams (IBD)** for selected subsystems. 

### Completed Work:
- **Created a System Context BDD** to establish an overview of system interactions.
- **Developed BDD and IBD for AMS**, detailing the core components such as:
  - **Flight Authorization Module** (handles flight approvals)
  - **No-Fly Zone Management** (manages restricted airspaces)
  - **Emergency Intervention** (handles emergency landing procedures)
  - **Real-Time Air Traffic Monitoring** (monitors aerial traffic and coordinates with ATC)
- **Developed BDD and IBD for GCS**, outlining its role in mission execution and UAV communication.
- **Created BDDs for PHS and UAV**, specifying their functional components and integration points.

These diagrams serve as the foundation for refining subsystem interactions, communication protocols, and system-wide functionalities. Further iterations will enhance details and incorporate integration requirements with external systems.

This md file was written with the help of AI.

### Diagrams:
![System Context BDD](images/SystemContext_BDD.png)
![AMS BDD](images/AMS_BDD.png)
![AMS IBD](images/AMS_IBD.png)
![GCS BDD](images/GCS_BDD.png)
![GCS IBD](images/GCS_IBD.png)
![PHS BDD](images/PHS_BDD.png)
![UAV BDD](images/UAV_BDD.png)