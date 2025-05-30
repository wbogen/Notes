**Version 1.0**
**Date: May 30, 2025**

# Table of Contents
[[#1. Introduction]] 
    [[#1.1. Document Purpose]] 
    [[#1.2. System Overview & Goals]] 
    [[#1.3. Scope]] 
    [[#1.4. Audience]] 
    [[#1.5. References]]
    
 [[#2. Guiding Requirements & Decisions]] 
     [[#2.1. Core Functional Requirements Driving Design]] 
     [[#2.2. Key Non-Functional Requirements & Constraints]]
    
[[#3. System Architecture]] 
    [[#3.1. Architectural Overview (Diagram & Text)]] 
    [[#3.2. Component Breakdown]] 
    [[#3.3. Component Interfaces]] 
    [[#3.4. Technology Stack (Existing & New)]]
    
[[#4. Data Design and Flow]] 
    [[#4.1. Data Flow Diagrams (DFDs)]] 
    [[#4.2. Logical Data Model & Database Schema Interaction]] 
    [[#4.3. Data Storage & Synchronization Mechanisms]]
    
[[#5. Detailed Design of Key Processes & Workflows]] 
    [[#5.1. Manual Batch & Experimental Data Logging (Tablet Workflow)]] 
    [[#5.2. Automated Functional Data Transfer & Verification (New Utility Workflow)]] 
    [[#5.3. Local Server Data Processing & Analysis]] 
    [[#5.4. Data Preparation for and Interaction with Third-Party ETL Process]] 
    [[#5.5. Data Querying, Reporting, and Export]] 
    [[#5.6. Notification Workflow for Subsequent Experimental Phases (Stretch Goal)]]
    
[[#6. User Access, Security, and Compliance]] 
    [[#6.1. User-Based Access Control for Querying]] 
        [[#6.1.1. BLDS System Roles and Permissions]] 
    [[#6.2. Data Security Considerations]] 
    [[#6.3. SEND Standard Compliance for Data Export]]
    
[[#7. Deployment Overview]]
    
[[#8. Glossary]]

## 1. Introduction

### 1.1. Document Purpose

This Focused Design Specification (FDS) provides a comprehensive technical blueprint for the Biology Lab Data System (BLDS). It details the chosen architecture, data flows, and key requirements decisions that shape the design, focusing on _what will be built and how it integrates with existing lab processes and infrastructure, including third-party managed services_. This document serves as the primary guide for the development, integration, and deployment of new system components and the utilization of existing ones.

### 1.2. System Overview & Goals

The Biology Lab Data System (BLDS) is designed to transform the current, largely manual, and fragmented data management practices into a cohesive, automated, and traceable digital ecosystem for biology lab experiments. Currently, batch records are recorded on printed paper and then manually copied into a digital format, or information is stored in physical notebooks. Functional recordings are written to local device storage, manually copied to external hard drives, which are then physically moved to other PCs to be manually copied again onto the network drive; these external drives are retained until analysis completion as a precaution against data corruption. Existing in-house analysis scripts run on Windows desktops and write outputs to network files, requiring significant further manual compilation and connection to experimental details.

The BLDS aims to replace these labor-intensive and error-prone processes by introducing streamlined digital workflows. This encompasses:

- **Digital Batch & Experimental Data Entry:** Direct recording of experimental procedures, batch records, and observations (e.g., cell culture progress) using _newly introduced tablets_ to modify standardized digital documents (e.g., Microsoft Word files) directly on the network drive, replacing paper records, physical notebooks, and subsequent manual digital copying.

- **Automated Functional Data Transfer & Verification:** Direct and verified transfer of functional data from experimental equipment to the network drive via a newly developed utility, eliminating the multi-step manual copying via external hard drives and reducing handling risks.

- **Integrated Data Processing & Analysis:** Migration and modernization of existing in-house analysis scripts to a local server environment. Automated processing of raw functional data into usable measurements with outputs directly feeding the ETL preparation stage, and updating GUI-based tools to interact with the central database for selections. This will automate the compilation and linking of analysis outputs with their corresponding experimental 

- **Centralized Data Consolidation:** Preparation of data for ingestion into a _newly established_ third-party managed off-site ETL database (PostgreSQL-based), utilizing a third-party managed ETL service. Standardized documents like batch records, if free of irregularities, will be made available for direct ingestion by the third-party ETL upon notification or via monitoring. A QA/Audit process will address irregularities.

- **Enhanced Data Access:** Providing flexible and secure data querying and export capabilities for researchers and for handoff to other teams/phases from the third-party managed database, primarily utilizing interfaces and export tools provided by the third party.


The primary goals are to significantly enhance data integrity and accuracy by minimizing manual handling and transcription errors; drastically improve operational efficiency by reducing labor-intensive tasks; ensure robust traceability from raw observations to final results; and streamline data handoffs between teams and processes.

### 1.3. Scope

This FDS covers the design of:

- Mechanisms for capturing and ingesting manual (tablet-based, modifying network drive documents) and automated (equipment-generated) experimental data, including the introduction of tablets and standardization of associated document 

- The migration, update, and deployment of existing in-house analysis scripts to a local server processing pipeline for functional data.

- Updates to existing GUI-based analysis tools.

- A QA/Audit process for manually entered data and documents failing ETL standardization.

- Processes and interfaces for preparing data for, and interacting with, the third-party managed ETL service and database, including mechanisms for notifying the ETL service of ready-to-ingest standardized documents.

- Interfacing with third-party provided tools for data querying and export from the third-party managed database, including the application of SEND format compliance rules to exported data.

- Integration points with existing infrastructure (network drives, S3 backups) and the third-party managed ETL and database environment.

- The new utility for automated data transfer from equipment PCs to the network drive.

- The design considerations for a _potential future_ notification service for subsequent experimental phases.


This document focuses on the design of _new or modified software components developed or implemented by the BLDS project_ and their interaction with existing systems and the newly established third-party managed ETL service and database. It does not cover the internal operational management of the third-party ETL service or database infrastructure, except for defining how BLDS will interface with them.

### 1.4. Audience

- Software Developers & Engineers
- System Architects
- QA & Testing Teams
- Project Managers
- Technical Leads
- Lab IT Personnel involved in integration
- Liaisons with the Third-Party Service Provider

### 1.5. References

- `srs_bio_lab_data_system_v1` (Software Requirements Specification for BLDTS)
    
- [Internal Document] Lab SOP-XXX: Tablet Usage and Data Synchronization (to be updated/created for new digital workflow)
    
- [Internal Document] Existing In-house Analysis Script Documentation (if available)
    
- [Internal Document] SOP for QA/Audit of Manually Entered Data (to be developed)
    
- [Third-Party Document] ETL Service and Database API/Query/Export Interface Specification & Schema Definition (for the new third-party managed environment), including specifications for direct ingestion notifications/monitoring.
    
- CDISC SEND Implementation Guide
    

## 2. Guiding Requirements & Decisions

This section highlights key requirements (functional and non-functional) that directly shape the system design. Requirement IDs may refer to the `srs_bio_lab_data_system_v1` document.

### 2.1. Core Functional Requirements Driving Design

- **FR1 (Tablet Data Entry):** Lab personnel shall use _newly provided tablets_ to access and modify standardized formatted text documents (e.g., Microsoft Word files, existing or in development, to be standardized) directly on a designated network drive for manual data recording. This is a _new workflow_ replacing paper records and manual digitization. (Derived from context; relates to REQ-BLDTS-F001, REQ-BLDTS-F004).

- **FR2 (Remote Document Update):** Tablet-accessible documents on the network drive may be updated outside the lab to inform next experimental steps, with changes saved directly to the network drive.

- **FR3 (Automated Functional Data Transfer):** A _newly developed utility_ (Automated Equipment Data Transfer Utility - Component A2) shall copy functional data from equipment PCs to the network drive, performing accuracy checks before deleting local PC files. This process shall run concurrently with data generation. (Replaces manual external drive transfer; relates to REQ-BLDTS-F009, REQ-BLDTS-F012).

- **FR4 (Local Data Processing & Script Migration):** Existing in-house analysis scripts shall be migrated from Windows desktops to a local server environment. The local server shall read raw functional data from the network drive and process it using these updated scripts to generate usable measurements. Outputs shall be directed to the data preparation stage for ETL, rather than network files. This includes automated analysis and error resolution. (Relates to REQ-BLDTS-F017).

- **FR5 (Manual Analysis Support & Enhancement):** Existing GUI-based tools for manual analysis shall be updated to read and write previous analysis selections/parameters from/to the Third-Party Managed Database to preserve context and improve usability. These tools will continue to support analysis of data not handled by automated methods. (Relates to REQ-BLDTS-F017).

- **FR6 (Handling Pre-Processed Data):** The system shall accommodate data already in a usable format or processed by other commercial software and uploaded to the network drive.

- **FR7 (Data Ingestion via Third-Party ETL):** Processed data, and standardized batch records, shall be prepared and made available for ingestion into the _newly established_ off-site third-party ETL database via a _third-party managed ETL service_. This service **shall be notified of or monitor specific locations for new, ready-to-ingest standardized documents (like batch records)** and may also pull other prepared data from the network drive/S3 or accept it via defined interfaces. Manual data entry capabilities may also be provided or integrated via this third-party service. (Relates to REQ-BLDTS-F025, REQ-BLDTS-F027).

- **FR8 (Diverse Querying via Third-Party Tools):** The system shall leverage query tools and export functionalities (e.g., to CSV) provided by the third-party managing the database and ETL pipeline. This includes enabling SQL queries (e.g., from R, Python) and integration with Power BI/Excel using these third-party mechanisms. The specific nature of programmatic access (e.g., REST API vs. direct DB connection via ODBC/JDBC) is TBD and dependent on third-party provisions. (Relates to REQ-BLDTS-F031, REQ-BLDTS-F032, REQ-BLDTS-F036).

- **FR9 (SEND Compliance Formatting):** The BLDS project will be responsible for developing a process to format data queried/exported from the third-party system into a SEND-compliant structure.

- **FR10 (Notification for Subsequent Phases -** _**Stretch Goal**_**):** The system _should eventually_ provide a mechanism to automatically notify relevant Research Associates or Scientists when data is ready for subsequent experimental phases (e.g., chemistry-related work). The initial operational plan will continue to rely on person-to-person communication for this handoff.

- **FR11 (QA/Audit of Manual Data & ETL Irregularities):** The system shall provide a mechanism for quality assurance (QA) review of all manually entered data (from tablets). Furthermore, if the Tablet Data Ingestion Service (A1) or the Third-Party ETL Service (C1) encounters documents that cannot be standardized or ingested due to irregularities, a process shall exist to flag this data, route it for audit, allow for manual correction/resolution, and re-submission into the pipeline.


### 2.2. Key Non-Functional Requirements & Constraints

- **NFR1 (Network Drive Reliance):** The system heavily relies on the existing local network drive for data staging, direct document modification via tablets, and inter-process communication. (Relates to REQ-BLDTS-HW001).
    
- **NFR2 (S3 Backup Integration):** The local network drive is backed up to an S3 bucket; this S3 bucket is also a source for the third-party ETL processes.
    
- **NFR3 (Third-Party Managed ETL and Database):** The primary data repository is a _newly established_ off-site ETL database (PostgreSQL), and the associated ETL service (Component C1) are managed by a third party. The BLDS development team has full access to define the schema and interact with the database for querying purposes (via third-party provided tools/interfaces), and will interface with the third-party ETL service according to defined specifications, including notification mechanisms for ready-to-ingest data. (Relates to REQ-BLDTS-SW001).
    
- **NFR4 (New BLDS Component Development & Migration):** The introduction of tablets, the Tablet Data Ingestion Service (A1), the Automated Equipment Data Transfer Utility (A2), and the migration/update of existing in-house analysis scripts to a server environment are key development/implementation aspects for the BLDS project. Components for preparing data for the third-party ETL, the QA/Audit workflow, and formatting exports for SEND compliance are also within BLDS scope.
    
- **NFR5 (User-Based Access Control):** Data access for querying via R, Python, etc., must be restricted based on user roles/permissions, enforced via third-party mechanisms or BLDS-developed layers if necessary. (Relates to REQ-BLDTS-F040, REQ-BLDTS-F041).
    
- **NFR6 (Data Traceability):** Source file traceability and other metadata must be captured and maintained within the database schema.
    
- **NFR7 (Concurrency):** The new Automated Equipment Data Transfer Utility (A2) shall support concurrent operation with ongoing data generation on equipment PCs. (Performance consideration, relates to REQ-BLDTS-NF002).
    

## 3. System Architecture

### 3.1. Architectural Overview (Diagram & Text)

The BLDS employs a distributed architecture, integrating on-premise components developed or migrated by the project with cloud-based storage (S3) and a third-party managed ETL service and database. Data flows from points of generation (newly introduced tablets modifying documents on network drive, equipment) through on-premise staging (network drive) and processing (local server hosting migrated scripts), then is made available to the third-party ETL service (potentially via notification for standardized documents) which loads it into the central database for long-term storage and querying. A QA/Audit workflow is integrated for manually entered data and ETL irregularities.

_(A high-level architectural diagram would be embedded here. This diagram would visually represent the flow: Lab Personnel/Tablets (New, accessing Network Drive) -> **Tablet Data Ingestion Service (New)** / QA Audit Process -> Network Drive (monitored/notified for 3rd Party ETL for standardized docs); Equipment -> Local PC -> **Automated Equipment Data Transfer Utility (New)** -> Network Drive; Network Drive -> S3 Backup; Network Drive -> Local Processing Server (hosting migrated scripts); Local Processing Server/S3/Manual Entry Points / QA Audit Process -> **Third-Party Managed ETL Service** -> **Third-Party Managed Database (New)**; Third-Party Managed Database -> Querying Tools/Interfaces & Export (Third-Party Provided); System -> Notification for Subsequent Phases (Stretch Goal).)_

**Key Architectural Principles:**

- **Decoupling:** Components interact via defined interfaces (e.g., file drops on network drive, API calls, database connections).
    
- **Leveraging Existing Infrastructure & Scripts:** Maximizes use of current network drives, S3, and adapts existing analysis scripts.
    
- **Third-Party Managed Core Repository & ETL:** The newly established third-party managed ETL service and database serve as the authoritative mechanism for data loading and as the repository for processed and consolidated data.
    
- **Automation of Manual Processes & Direct Digital Entry:** Key new components and workflows, such as tablet-based direct modification of network drive documents, the Tablet Data Ingestion Service, and the Automated Equipment Data Transfer Utility, are designed to replace current manual, error-prone steps.
    
- **Efficient Handoff for Standardized Data:** Standardized documents (e.g., batch records) can be directly ingested by the third-party ETL upon notification or monitoring, minimizing intermediate BLDS processing if no irregularities exist.
    
- **Integrated QA/Audit Loop:** Ensures quality of manually entered data and provides a resolution path for data that fails automated ingestion or standardization.
    

### 3.2. Component Breakdown

The BLDS comprises the following logical sub-systems and their components:

- **A. Data Capture & Ingestion Sub-system (BLDS Project Developed/Implemented):**
    
    - **A1. Tablet Data Ingestion Service:**
        
        - This is a _new component_ to be developed as part of the BLDS. It supports the new workflow where users on tablets directly modify standardized formatted text documents (e.g., Microsoft Word) stored on the network drive.
            
        - _Responsibilities:_ Monitors network drive locations for new/updated documents. Parses metadata from document structure/content. Prepares batch records and similar standardized documents in a format ready for direct ingestion by the Third-Party ETL Service. If documents are validated and free of irregularities, places them in a designated location and/or triggers a notification to the Third-Party ETL Service. Documents with irregularities are flagged and routed to the QA/Audit Process (part of B3.3).
            
    - **A2. Automated Equipment Data Transfer Utility:**
        
        - This is a _new component_ to be developed as part of the BLDS. It replaces the current manual process of copying data to external hard drives and then to the network drive.
            
        - _Responsibilities:_ Installs/runs on equipment PCs (or a connected utility PC). Monitors for new functional data files, copies them to a designated "incoming" area on the Network Drive, performs accuracy checks (e.g., checksum comparison), and if successful, deletes the original file from the local PC storage to manage space. Logs all operations and errors.
            
    - **A3. Pre-Processed Data Ingestion Service:**
        
        - Monitors network drive for data from commercial software or already in usable format.
            
        - Validates and registers data for preparation for ETL by B3.4.
            
- **B. Local Data Staging & Processing Sub-system (BLDS Project Developed/Migrated):**
    
    - **B1. Network Drive File System:** Central staging area for raw and intermediate data, including locations monitored by the Third-Party ETL Service and accessed by tablets.
        
    - **B2. S3 Synchronization Service:** (Likely an existing IT-managed process) Ensures network drive content is backed up/replicated to S3.
        
    - **B3. Local Data Processing Server:**
        
        - **B3.1. Automated Analysis Engine:** Hosts and executes _migrated and updated in-house analysis scripts_. Watches network drive for new raw functional data, applies these scripts to generate usable measurements. Outputs are directed to the Data Preparation for ETL Module (B3.4).
            
        - **B3.2. Manual Analysis Tool Interface:** Existing GUI-based tools, _to be updated by BLDS project_, to allow users to select data from the network drive, process it, and save results back. Crucially, these tools will be enhanced to read/write previous analysis selections/parameters from/to the Third-Party Managed Database to maintain context.
            
        - **B3.3. QA/Audit & Processing Error Handling Module:** Logs processing errors from B3.1, B3.2. Provides a workflow and interface for QA review of all manually entered data (from A1). Manages irregularities flagged by A1 (for tablet documents) or by the Third-Party ETL Service (C1) for documents that failed standardization/ingestion. Allows authorized users to audit, correct/annotate, and re-submit data for processing or ETL.
            
        - **B3.4. Data Preparation for ETL Module:** Consolidates and formats processed data from various sources (automated analysis, manual analysis, pre-processed data, and tablet documents with irregularities that have passed QA/resolution via B3.3) into structures suitable for ingestion by the Third-Party ETL Service. This may involve creating manifest files or placing data in specific formats/locations for scheduled pickup or API-based handoff.
            
- **C. Third-Party Managed ETL & Database Sub-system:**
    
    - **C1. Third-Party Managed ETL Service:**
        
        - This service is _provided and managed by the third party_.
            
        - _Responsibilities (as perceived by BLDS):_ Is notified of or actively monitors designated locations (Network Drive/S3) for new, standardized, ready-to-ingest documents (e.g., batch records from A1) for direct ingestion if free of irregularities. For other data, it extracts data prepared by the BLDS's B3.4 module (from network drive/S3 or via API). Performs ETL operations defined by the third party or in collaboration, handles data validation during its process, and loads data into the database. Flags documents with irregularities for review by the BLDS QA/Audit Module (B3.3). Provides mechanisms for monitoring its jobs and handling errors within its scope.
            
    - **C2. Third-Party Managed Database:**
        
        - This is the _newly established central data repository_ for BLDS, managed by a third party, using PostgreSQL. It will store Experiments, Systems, Data, and Metadata tables/table categories as defined by the BLDS project (in collaboration with the third party). The third party is responsible for the database infrastructure, uptime, and operational management.
            
- **D. Querying, Reporting & Export Sub-system (Primarily Third-Party Provided, with BLDS formatting layer for SEND):**
    
    - **D1. Programmatic Access (via Third-Party):** The third party will provide mechanisms (e.g., ODBC/JDBC drivers, potentially APIs) for programmatic queries (e.g., from R, Python) to their managed database. The BLDS project will not develop a separate REST API for this unless deemed absolutely necessary.
        
    - **D2. Data Export Functionality (Third-Party Provided):** Core data export capabilities (e.g., to CSV) are expected to be part of the query tools provided by the third party.
        
    - **D3. BI Tool Integration (via Third-Party):** Connections from Power BI and Excel to the Third-Party Managed Database will utilize mechanisms provided or recommended by the third party.
        
    - **D4. Third-Party Query Tool Interface:** (Provided by the database vendor/third-party) Users can directly use this tool (standalone application or web interface as provided) to query the new database.
        
    - **D5. SEND Compliance Formatting Layer (BLDS Developed):** A BLDS-developed process that takes data exported via third-party tools (D2) and transforms/formats it into SEND-compliant structures.
        
- **E. Notification Sub-system (**_**Future Consideration/Stretch Goal**_ **- BLDS Project Developed if pursued):**
    
    - **E1. Notification Service for Subsequent Phases (**_**Stretch Goal**_**):** _If implemented as a future enhancement_, this service would monitor experiment completion status (e.g., based on data uploads or flags in the database) and send automated notifications (e.g., email, dashboard alert) to relevant RAs/Scientists for subsequent work. The initial operational plan relies on existing person-to-person communication for this handoff.
        

### 3.3. Component Interfaces

- **Tablet Users <-> Network Drive:** Direct file access/modification (e.g., MS Word docs) via tablet OS/apps.
    
- **Tablet Data Ingestion Service (A1) <-> Network Drive:** File system read/monitoring.
    
- **Tablet Data Ingestion Service (A1) -> Staging for Third-Party ETL Service:** File drop to specific monitored/notified locations on Network Drive/S3 for standardized, error-free documents.
    
- **Tablet Data Ingestion Service (A1) -> QA/Audit & Processing Error Handling Module (B3.3):** Handoff of data from tablet documents with irregularities.
    
- **Automated Equipment Data Transfer Utility (A2) -> Network Drive:** File system operations (write).
    
- **Network Drive -> Local Processing Server (Automated Analysis Engine B3.1):** File system read operations.
    
- **Local Processing Server (Automated Analysis Engine B3.1) -> Data Preparation for ETL Module (B3.4):** Internal data handoff.
    
- **Manual Analysis Tool Interface (B3.2) <-> Network Drive:** File system read/write for data files.
    
- **Manual Analysis Tool Interface (B3.2) <-> Third-Party Managed Database (C2):** Database connection for reading/writing analysis selections/parameters.
    
- **QA/Audit & Processing Error Handling Module (B3.3) <-> Users (via UI):** Interface for reviewing, correcting, and approving data.
    
- **QA/Audit & Processing Error Handling Module (B3.3) -> Data Preparation for ETL Module (B3.4) / Staging for Third-Party ETL:** Handoff of resolved data.
    
- **Third-Party ETL Service (C1) -> QA/Audit & Processing Error Handling Module (B3.3):** Notification/flagging of documents that failed ETL standardization.
    
- **Local Processing Server (Data Preparation Module B3.4) -> Staging for Third-Party ETL Service:** File system write operations to designated locations (Network Drive/S3), or API calls if the third-party ETL service provides an ingestion API for this type of data.
    
- **Network Drive <-> S3 Bucket:** File synchronization protocols (managed by existing IT tools).
    
- **Third-Party ETL Service (C1) -> Third-Party Managed DB (C2):** Internal interface within the third-party environment.
    
- **Third-Party ETL Service (C1) <-> S3 Bucket / Network Drive:** File system read access (as defined by interface agreement, including monitored locations for direct ingestion).
    
- **Querying Interfaces (D1, D3, D4 - Third-Party) -> Third-Party Managed DB (C2):** Mechanisms provided by the third party.
    
- **SEND Compliance Formatting Layer (D5) -> Input from Third-Party Export (D2):** Data file/stream.
    
- **Notification Service (if implemented) -> Database (C2):** SQL queries to check status.
    
- **Notification Service (if implemented) -> Users:** Email (SMTP), internal messaging system API.
    

### 3.4. Technology Stack (Existing & New)

- **Existing (Infrastructure/Tools Leveraged or Migrated):**
    
    - **Network Drive:** Standard file server (e.g., Windows Server, NAS).
        
    - **S3:** AWS S3.
        
    - **In-house Analysis Scripts:** (Specify language, e.g., Python, R, Matlab) currently on Windows desktops, _to be migrated to Local Processing Server and updated_.
        
    - **GUI-based Analysis Tools:** (Specify technology if known) _to be updated for DB interaction_.
        
    - **Commercial Analysis Software:** (Specify names if relevant to integration).
        
    - **Formatted Text Document Templates (for Tablets):** Existing or in-development (e.g., Microsoft Word .docx), _to be standardized by BLDS project_.
        
- **Third-Party Provided (for New Database & ETL):**
    
    - **ETL Service Environment (C1):** (e.g., Informatica, Talend, AWS Glue, Azure Data Factory, custom solution provided by third party), including mechanisms for notification/monitoring of ready-to-ingest files.
        
    - **Database Environment (C2):** PostgreSQL (chosen for potential complex data representation; final schema TBD by third party) and its associated query tool(s), export functionalities, and access methods (e.g., ODBC/JDBC drivers, APIs).
        
- **New/To Be Developed or Implemented by BLDS Project:**
    
    - **Tablets:** Specific models and OS (e.g., Windows, Android, iOS capable of network drive access and running Microsoft Word or equivalent word processor) to be selected and provisioned.
        
    - **Tablet Data Ingestion Service (A1):** Python (with libraries like `watchdog`), Java, or .NET service running on a server monitoring the network drive. Includes logic for validation and routing for direct ETL or local processing/QA.
        
    - **Software/Apps for Tablets:** Standard word processors (e.g., Microsoft Word) for editing documents directly on the network drive.
        
    - **Automated Equipment Data Transfer Utility (A2):** Rust utility. Should be lightweight and runnable on equipment PCs or connected utility PCs.
        
    - **Local Processing Server Environment (for B3):** Linux or Windows Server, with necessary runtimes for migrated scripts (e.g., Python, R environments).
        
    - **QA/Audit & Processing Error Handling Module (B3.3 UI):** Web application (e.g., Python Flask/Django, Node.js with React/Vue) for reviewing and resolving data issues.
        
    - **Data Preparation for ETL Module (B3.4):** Scripts or services (likely Python) for data aggregation and formatting.
        
    - **SEND Compliance Formatting Layer (D5):** Transformation logic (e.g., Python scripts) to convert data exported from the third-party system into SEND format.
        
    - **Notification Service (E1) (**_**Stretch Goal**_**):** Python script, or integrated with other BLDS services, if implemented.
        
    - **Web UIs (for admin tasks for BLDS components, QA/Audit):** Standard web technologies (HTML, CSS, JavaScript with a backend framework).
        

## 4. Data Design and Flow

### 4.1. Data Flow Diagrams (DFDs)

_(This section would contain the actual DFDs. Below are descriptions of what they'd show.)_

- **Context Diagram (Level 0):**
    
    - **External Entities:** Lab Personnel (with Tablets accessing Network Drive), Experimental Equipment (PCs), **Automated Equipment Data Transfer Utility Admin/Monitor**, Local Processing Server Admin, QA/Audit Users, **Third-Party Service Provider (managing ETL & DB)**, Data Analysts/Query Users, BI Tools, RAs/Scientists (for notifications).
        
    - **System:** "Biology Lab Data System (BLDS Developed & Migrated Components)" as a single process.
        
    - **Major Data Flows:** Modified Tablet Documents (from Network Drive), Raw Functional Data, Processed & Prepared Data (to Third-Party ETL), QA/Audit Data Resolutions, Query Requests (to Third-Party DB via defined interfaces), Query Results (from Third-Party), Notifications (if implemented), Configuration Data.
        
- **Level 1 DFD (Illustrating flow between sub-systems from 3.2):**
    
    - **Processes (BLDS Developed/Managed):** Data Capture & Ingestion (A1), Automated Equipment Data Transfer (A2), Pre-Processed Data Ingestion (A3), Local Data Processing (B3.1, B3.2), QA/Audit & Error Resolution (B3.3), Data Prep for ETL (B3.4), SEND Compliance Formatting (D5), Notification (Stretch Goal - E1).
        
    - **External Processes (Third-Party):** Third-Party ETL Service (C1), Third-Party Database (C2), Third-Party Query & Export Tools.
        
    - **Data Stores:** Network Drive, S3 Bucket, Third-Party Managed Database (Experiments, Systems, Data, Metadata table categories).
        
    - **Flows:**
        
        - `Tablet Document (on Network Drive)` -> **Tablet Data Ingestion Service (A1)**.
            
        - A1 -> `Standardized Batch Record Data (for direct ETL if clean)` -> Staging for Third-Party ETL (monitored/notified).
            
        - A1 -> `Tablet Data (with irregularities)` -> QA/Audit & Error Resolution Module (B3.3).
            
        - `Equipment Output File` -> **Automated Equipment Data Transfer Utility (A2)** -> `Raw Functional Data File` -> Network Drive.
            
        - Network Drive -> `Raw Data for Processing` -> Local Data Processing (incl. migrated scripts B3.1) -> `Processed Data for ETL Prep` -> Data Preparation for ETL Module (B3.4).
            
        - QA/Audit & Error Resolution Module (B3.3) -> `Resolved Data` -> Data Preparation for ETL Module (B3.4) / Staging for Third-Party ETL.
            
        - Data Preparation for ETL Module (B3.4) -> `Prepared Data Package` -> Staging Area (Network Drive/S3 for Third-Party ETL pickup).
            
        - Staging Area (Network Drive/S3) -> `Data for Ingestion` -> **Third-Party ETL Service (C1)** -> `Structured Data Inserts` -> Third-Party Managed Database.
            
        - **Third-Party ETL Service (C1)** -> `ETL Standardization Failures` -> QA/Audit & Error Resolution Module (B3.3).
            
        - Third-Party Managed Database -> `Query Data` -> Third-Party Query/Export Tools -> `Exported Data`.
            
        - `Exported Data` -> SEND Compliance Formatting Layer (D5) -> `SEND Formatted Data`.
            
        - `Experiment Status` -> Notification Service (Stretch Goal) -> `RA/Scientist Alert`.
            
        - `Analysis Selections/Parameters` <-> Manual Analysis Tool Interface (B3.2) <-> Third-Party Managed Database (C2).
            

### 4.2. Logical Data Model & Database Schema Interaction

The BLDS will define (in collaboration with the third party) and interact with the schema of the new third-party managed database. The underlying database technology is **PostgreSQL**, selected for its capability to handle complex data representations that may arise from the diverse experimental data and the detailed, multi-table schema structure within each category. The final schema design, leveraging PostgreSQL's features (e.g., JSONB, arrays, custom types if needed), will be proposed by the third party in consultation with the BLDS team.

The schema will be organized into logical categories, where each category may represent a series of related, more granular tables designed to accommodate the diversity of experimental data.

- **General Principle:** Instead of single monolithic tables for "Experiments," "Systems," "Data," and "Metadata," these are considered **categories**. Within each category, there will likely be multiple specific tables. For example:
    
    - The "Data" category might include separate tables for different types of functional readouts (e.g., `CellViabilityData`, `ProteinExpressionData`, `ImagingMetricsData`), each with columns for their unique, relevant metrics.
        
    - The "Experiments" category might have a core `Experiments` table with common details, but also related tables for specific experimental designs or complex condition tracking.
        
    - Batch records, captured in tablet documents, will vary in content based on the experiment type. The ETL process (or direct ingestion for standardized forms) must map this variable information into the appropriate tables within these categories.
        
- **Experiments Table Category:**
    
    - _Purpose:_ Stores metadata relating to the experimental plan, design, and execution.
        
    - _Potential Core Attributes (in a central `Experiments` table):_ ExperimentID (PK), ExperimentName, ProjectID, StartDate, EndDate, ExperimentOperatorID, ExperimentalHypothesis.
        
    - _Related Tables/Considerations:_ Specific tables for different experimental types might capture unique parameters, conditions, or planned timepoints. Batch record information (materials, specific procedural notes) will be linked here or to specific data entries.
        
- **Systems Table Category:**
    
    - _Purpose:_ Stores system configuration details, biological system details (e.g., cell lines), and equipment information relevant to an experiment.
        
    - _Potential Core Attributes (in a central `Systems` or `BiologicalSystems` table):_ SystemID (PK), SystemName, CellLineID, PassageNumber.
        
    - _Related Tables/Considerations:_ Separate tables for `EquipmentUsed` (EquipmentID, HardwareVersion, SoftwareVersion, CalibrationDate) linked to experiments or specific data acquisition events.
        
- **Data Table Category:**
    
    - _Purpose:_ Stores the actual experimental results, functional readouts, dosage conditions, phenotypes, etc. This is the most likely category to have many specialized tables.
        
    - _Potential Core Attributes (common across many data tables, or in a linking table):_ DataPointID (PK), ExperimentID (FK), SystemID (FK), Timepoint, RawDataFileRef.
        
    - _Specialized Tables:_ As mentioned, tables like `CellViabilityData` (with columns for `%Viability`, `MeasurementMethod`), `GeneExpressionData` (with `GeneID`, `FoldChange`, `PValue`) would exist, each tailored to specific metrics. Dosage conditions and observed phenotypes would be linked appropriately.
        
- **Metadata Table Category:**
    
    - _Purpose:_ Stores source file traceability, audit information, analysis parameters, and any other contextual information not captured in the primary data tables.
        
    - _Potential Core Attributes (in a central `AuditMetadata` or `FileTraceability` table):_ MetadataID (PK), AssociatedTable, AssociatedRecordID, SourceFileName, SourceFilePath (Network Drive/S3), IngestionTimestamp, ProcessingLogRef, VersionNumber, UserComments, QAStatus, ResolvedBy, ResolutionDate.
        
    - _Related Tables/Considerations:_ A specific table or JSONB field within a metadata table might store `AnalysisSelectionParameters` for the updated GUI tools (B3.2).
        

**Data Linking Strategy:** The data prepared by BLDS's Local Data Processing Server (specifically B3.4) or by A1 for direct ingestion must contain all necessary identifiers and contextual information for the Third-Party ETL Service (C1) to correctly populate these potentially numerous and interlinked tables. The schema design will require careful planning with the third-party provider to ensure flexibility and accurate data representation.

### 4.3. Data Storage & Synchronization Mechanisms

- **Tablets (New):** Users on tablets will directly access and modify standardized Microsoft Word (or equivalent) documents stored on the Network Drive via appropriate file access mechanisms and network connectivity. Local caching on tablets might occur via the word processing application, but the authoritative copy being modified is on the Network Drive.
    
- **Equipment PCs:** Local temporary storage for raw functional data before being processed by the **Automated Equipment Data Transfer Utility (A2)**.
    
- **Network Drive:** Primary staging area for:
    
    - Standardized documents (e.g., MS Word batch records) directly modified by users on tablets. Some of these, if validated by A1 and error-free, are made available in locations where the Third-Party ETL service is notified or monitors for direct ingestion.
        
    - Raw functional data copied by the **Automated Equipment Data Transfer Utility (A2)**.
        
    - Processed functional data from the local server (output of migrated scripts), prepared by B3.4 for the third-party ETL.
        
    - Analysis logs, QA/Audit temporary files.
        
- **S3 Bucket:**
    
    - Backup of the Network Drive.
        
    - Staging area where the Third-Party ETL Service is notified of or monitors for new, ready-to-ingest standardized documents (like batch records). For other data, it serves as a staging area for data prepared by B3.4.
        
    - Synchronization between Network Drive and S3 is managed by existing IT processes.
        
- **Local Processing Server:** May have temporary local storage during active processing, but persistent processed output (prepared for ETL) goes to the Network Drive or S3.
    
- **Third-Party Managed Database:** Off-site PostgreSQL database, newly established for BLDS and managed by a third party. The ultimate repository for structured, processed, and linked data, organized into the described table categories.
    

## 5. Detailed Design of Key Processes & Workflows

### 5.1. Manual Batch & Experimental Data Logging (Tablet Workflow)

1. Lab personnel use _newly provisioned tablets_ with network access to directly open and modify pre-defined formatted Microsoft Word (or equivalent) document templates stored on a designated Network Drive. These templates are standardized (from existing/in-development documents as part of BLDS).
    
2. Data (observations, materials used, procedural steps, batch info) is manually entered directly into these documents on the Network Drive throughout the experiment, including updates made outside the lab. This replaces paper forms and notebooks. Changes are saved directly to the network copy.
    
3. The **Tablet Data Ingestion Service (A1)** monitors these document locations on the Network Drive for changes (e.g., based on file modification timestamps, or a trigger mechanism).
    
4. Upon detecting a new or updated file, A1 parses key metadata (e.g., Experiment ID, Batch ID, User, Timestamp from document content or filename) and validates the document against standardization rules for direct ETL ingestion.
    
5. Handling of Parsed Tablet Data:
    
    a. If the document is standardized and passes initial validation (no irregularities): The document reference (and potentially extracted key data) is placed in a specific, designated location/queue on the Network Drive or S3. The Third-Party ETL Service (C1) is then notified or actively monitors this for direct ingestion.
    
    b. If irregularities are detected by A1: The document and an error report are routed to the QA/Audit & Processing Error Handling Module (B3.3) for local review and resolution.
    
6. An entry is made in the **Metadata Table Category** in the Third-Party Managed Database (via the ETL process or after QA resolution) recording the file's status, location, and basic parsed metadata.
    

### 5.2. Automated Functional Data Transfer & Verification (New Utility Workflow)

1. Experimental equipment generates functional data files on its local PC storage.
    
2. The Automated Equipment Data Transfer Utility (A2), running on the equipment PC or a connected utility PC:
    
    a. Monitors designated local folders for new data files based on configurable patterns (e.g., file extension, naming convention).
    
    b. Upon detecting a new file, copies it to a designated "incoming" raw functional data area on the Network Drive.
    
    c. Performs an accuracy check (e.g., checksum comparison, file size verification) between the source file on the PC and the copied file on the Network Drive.
    
    d. If verification is successful, deletes the original file from the equipment PC's local storage to manage disk space. This deletion may be configurable (e.g., immediate delete, delete after X hours/days, or move to a local archive folder).
    
    e. If verification fails, logs an error, does not delete the source file, and potentially retries the copy N times or sends an alert to a designated administrator/user group.
    
    f. Logs all successful transfers, verification results, and errors to a local log file on the PC and/or a central logging mechanism if available.
    
3. This utility is designed to run concurrently with ongoing data generation, minimizing disruption to experiments.
    
4. Once data is successfully on the Network Drive, these files are available for the **Local Data Processing Server (B3)**.
    

### 5.3. Local Server Data Processing & Analysis

1. The **Automated Analysis Engine (B3.1)** on the Local Processing Server (hosting migrated in-house scripts) monitors the Network Drive for new raw functional data files (from 5.2) and pre-processed/commercial software outputs (from A3).
    
2. Based on file type, source equipment (derived from filename/path or metadata), or configuration rules, it triggers specific _updated analysis scripts_.
    
3. Scripts perform calculations, transformations, and generate usable measurements. Output is directed to the "processed_staging" area for the Data Preparation for ETL Module (B3.4).
    
4. Processed data and analysis logs are written to a designated "processed_staging" area on the Network Drive.
    
5. QA/Audit & Processing Error Handling Module (B3.3):
    
    a. Logs and manages errors from automated analysis (B3.1) and manual analysis (B3.2).
    
    b. Provides a QA workflow for all manually entered tablet data (flagged by A1 or proactively reviewed). Users (e.g., Scientists, designated RAs) review data via a UI, make corrections if necessary, and approve it.
    
    c. Manages documents flagged by A1 or the Third-Party ETL (C1) for irregularities or failure to standardize. Provides an interface for users to view issues, correct source documents (on network drive) or metadata, and re-trigger ingestion/preparation.
    
    d. Tracks resolution status and audit trails for all QA/corrected data.
    
6. Manual Analysis (B3.2): For data requiring manual intervention or not supported by automated pipelines:
    
    a. Users access updated GUI-based tools.
    
    b. Users select raw data files from the Network Drive.
    
    c. Tools allow users to make selections/set parameters. These selections/parameters shall be read from and written to the Metadata Table Category (or a dedicated table) in the Third-Party Managed Database to persist user choices across sessions and for audit purposes.
    
    d. Perform analysis using the tools.
    
    e. Save processed results and any relevant parameters/logs to the "processed_staging" area on the Network Drive.
    
7. The **Data Preparation for ETL Module (B3.4)** takes inputs from "processed_staging" (both automated and manual outputs) and QA-approved tablet documents/data from B3.3. It consolidates, validates, and formats this data according to the specifications required by the **Third-Party Managed ETL Service (C1)**.
    
8. Entries in the **Metadata Table Category** (within the Third-Party Managed Database) are updated via the Third-Party ETL process to reflect processing status and link to processed files and logs.
    

### 5.4. Data Preparation for and Interaction with Third-Party ETL Process

This section describes two primary pathways for data to reach the Third-Party Managed ETL Service:

**Path 1: Direct Ingestion of Standardized Documents (e.g., Batch Records)**

1. The **Tablet Data Ingestion Service (A1)** identifies standardized documents (e.g., batch records from network drive) that pass initial validation and are free of irregularities.
    
2. These documents (or their directly ingestible data representation) are placed in a specific, designated location on the Network Drive or S3.
    
3. A notification mechanism (e.g., API call to a third-party endpoint, message queue, trigger file) informs the **Third-Party Managed ETL Service (C1)** of the availability of new data in this location. Alternatively, C1 actively monitors this location.
    
4. C1 ingests these files directly, performing its own validation and loading into the Third-Party Managed Database (C2). If C1 finds irregularities, it flags the document for review via the BLDS **QA/Audit & Processing Error Handling Module (B3.3)**.
    

**Path 2: BLDS Data Preparation for Other Data**

1. The **Data Preparation for ETL Module (B3.4)** within the Local Data Processing Sub-system processes:
    
    - Functional data from the Automated Analysis Engine (B3.1).
        
    - Results from Manual Analysis Tools (B3.2).
        
    - Pre-processed data (A3).
        
    - Tablet documents that had irregularities and were resolved through the QA/Audit Module (B3.3).
        
2. B3.4 consolidates, performs final validation, and transforms this data into the specific file formats and structures required by the **Third-Party Managed ETL Service (C1)**.
    
3. This prepared data package is placed in a designated staging area (Network Drive/S3) for pickup by C1, or transmitted via an agreed API.
    

**Interaction with Third-Party ETL Service (C1):**

1. The Third-Party Managed ETL Service (C1) extracts data from the designated locations (for both direct ingestion and prepared packages).
    
2. It performs its own transformations, validations, and loading operations into the Third-Party Managed Database (C2).
    
3. Close coordination with the third-party provider is essential for:
    
    - Specifications for directly ingestible document formats and the notification/monitoring mechanism.
        
    - Data format and structure specifications for data prepared by B3.4.
        
    - Mechanisms for triggering/scheduling ETL jobs.
        
    - Error reporting and resolution processes for issues identified _during the third-party ETL process_ (feeding back to B3.3).
        
    - Monitoring interfaces for ETL job status.
        
4. Manual data entry or correction, if needed, will likely interface with the Third-Party ETL Service's provided tools or feed into the BLDS's B3.4 module (after QA via B3.3) for preparation.
    
5. The Third-Party ETL Service updates the **Metadata Table Category** in C2 with ETL completion status, timestamps, and references.
    

### 5.5. Data Querying, Reporting, and Export

1. **Primary Query Interface (Third-Party Provided):** Users will primarily interact with the Third-Party Managed Database using query tools and interfaces (e.g., standalone application, web interface) provided by the third-party vendor managing the ETL/DB pipeline. The specific nature of these tools is TBD by the third party. These tools are expected to include core data export functionalities (e.g., to CSV).
    
2. Programmatic Access (via Third-Party Mechanisms):
    
    a. Access for R, Python scripts will leverage mechanisms provided by the third party (e.g., ODBC/JDBC drivers, potentially a third-party API).
    
    b. The BLDS project may develop a thin Programmatic Access Layer (D1) if needed to simplify or standardize access for internal scripts, but this depends on the robustness and ease of use of third-party provisions.
    
    c. Authentication and authorization will be managed via third-party mechanisms.
    
3. BI Tool Integration (via Third-Party Mechanisms):
    
    a. Power BI and Excel will connect to the Third-Party Managed DB using standard connectors (ODBC/JDBC) or specific connectors if provided by the third party.
    
4. SEND Compliance Formatting Layer (BLDS Developed):
    
    a. This BLDS-developed process (D5) will take data exported/queried from the Third-Party Managed Database (using third-party tools/interfaces) and apply necessary transformations to structure it according to the CDISC SEND standard.
    

### 5.6. Notification Workflow for Subsequent Experimental Phases (Stretch Goal)

_(This workflow describes a stretch goal for future system enhancement. The initial system deployment will rely on existing person-to-person communication for handoff for subsequent work, which may involve chemistry or other phases.)_

1. _If implemented_, the **Notification Service (E1)** would periodically query the Third-Party Managed Database or monitor flags set by the Third-Party ETL Service.
    
2. It would check for experiments/batches that have reached a "completed" or specific milestone state.
    
3. Upon detecting this, it would trigger a notification to designated RAs/Scientists (e.g., via email, dashboard alert) responsible for the next phase of work.
    
4. The notification would include relevant identifiers (e.g., Experiment ID, Batch ID) for them to retrieve the data.
    

## 6. User Access, Security, and Compliance

### 6.1. User-Based Access Control for Querying

- Authentication shall be required for all query interfaces to the Third-Party Managed Database, managed via third-party mechanisms.
    
- Role-Based Access Control (RBAC) will be implemented, defined in collaboration with the third party. Roles will be defined for _querying the final database_.
    
- Permissions associated with these roles will determine:
    
    - Which tables/views can be accessed.
        
    - Which specific data rows can be accessed (e.g., based on project, data sensitivity).
        
    - This will be enforced primarily at the database level by the third party, or through their provided query interfaces.
        

### 6.1.1. BLDS System Roles and Permissions

This section defines user roles and permissions for interacting with the _BLDS-developed/managed components and workflows_.

- **Research Associate (RA):**
    
    - Performs lab work and records data in batch records/experimental documents using tablets (modifying files on the network drive).
        
    - Operates recording equipment and ensures data is captured for the Automated Equipment Data Transfer Utility (A2).
        
    - May perform some GUI-based analysis using updated tools (B3.2), including saving/retrieving selections from the database.
        
    - May participate in the QA/Audit process (B3.3) for data they generated, if designated.
        
    - May perform some querying from the database via third-party tools for further statistical analysis under guidance.
        
- **Scientist:**
    
    - May perform some lab work and data recording similar to RAs.
        
    - Designs experimental plans.
        
    - Fills in/updates batch records or experimental plans (tablet documents on network drive), potentially outside the lab, to inform next steps.
        
    - Performs some GUI-based analysis using updated tools (B3.2), including saving/retrieving selections.
        
    - Will perform querying from the database via third-party tools for further statistical analysis and interpretation.
        
    - Manages RAs and may have oversight/approval roles in the QA/Audit process (B3.3).
        
- **Engineer:**
    
    - May perform some specialized analysis and exploratory work using functional recordings, potentially outside standard experimental processes.
        
    - May utilize GUI-based tools (B3.2) or advanced querying for their analysis.
        
    - May be involved in troubleshooting data processing issues for exploratory data.
        
- **Software Engineer (SE):**
    
    - A subset of Engineer responsible for maintaining the operational state of all BLDS-developed components (A1, A2, B3.1, B3.2-updates, B3.3, B3.4, D5, E1-if implemented).
        
    - Monitors BLDS system logs and performance.
        
    - Performs complex querying from the database for troubleshooting or system analysis.
        
    - Has increased access permissions to BLDS components and potentially to staging areas to resolve data errors or resubmit data through the QA/Audit (B3.3) or Data Preparation (B3.4) modules.
        
    - Will work with the third-party provider for technical support regarding the ETL service and database when issues impact BLDS integration.
        
- **Operations:**
    
    - Has elevated visibility into the database system (via third-party tools) for viewing logs and general usage patterns (business intelligence layer).
        
    - Will perform some querying primarily at the BI layer for operational insights.
        
    - Unlikely to need to make direct writes to the database; will work with SEs or designated data stewards if data correction at the database level is absolutely necessary (though primarily corrections should occur via the QA/Audit process before ETL).
        

### 6.2. Data Security Considerations

- **Data in Transit:**
    
    - Tablet access to Network Drive: Secure protocols (e.g., SMB3 with encryption, VPN if remote).
        
    - Network Drive to S3: HTTPS.
        
    - Client to Query Interfaces (Third-Party or BLDS API): HTTPS.
        
    - Data handoff to Third-Party Managed ETL Service: Secure mechanisms as defined by the third party (e.g., SFTP, HTTPS for API calls, secure S3 access, secure notification channel).
        
    - Third-Party ETL Service to Third-Party Managed DB: Internal to third-party; assumed secure.
        
- **Data at Rest:**
    
    - Network Drive: Access controlled by file system permissions. Consider drive-level encryption if sensitive.
        
    - S3 Bucket: Server-Side Encryption (SSE-S3 or SSE-KMS). Bucket policies and IAM roles to restrict access.
        
    - Third-Party Managed Database: Encryption at rest capabilities of the PostgreSQL database (provided by third party) should be enabled. Access controlled by database user credentials.
        
- **Application Security:**
    
    - Web interfaces (QA/Audit UI, admin UIs for BLDS components) must be protected against common vulnerabilities (OWASP Top 10).
        
    - API endpoints (if any developed by BLDS) must be secured (e.g., input validation, authentication, authorization).
        
    - The **Automated Equipment Data Transfer Utility (A2)** must operate with appropriate permissions on equipment PCs and ensure its configuration is secure.
        

### 6.3. SEND Standard Compliance for Data Export

- The **SEND Compliance Formatting Layer (D5)**, developed by BLDS, will contain specific logic to transform data queried and exported from the Third-Party Managed Database (via its interfaces) into SEND-compliant datasets.
    
- This involves mapping existing fields from the potentially complex schema (see 4.2) to SEND domains (e.g., DM, VS, EX, LB).
    
- Controlled terminology required by SEND will need to be managed, potentially within the BLDS or by referencing external controlled terminology sources.
    
- The output will be structured files (e.g., XPT - SAS Transport Format, or CSVs organized by domain) as per SEND IG.
    

## 7. Deployment Overview

- **Automated Equipment Data Transfer Utility (A2):** Deployed on individual equipment PCs or connected utility PCs.
    
- **Tablet Data Ingestion Service (A1), Local Processing Server (hosting migrated scripts and B3 components including QA/Audit UI backend), SEND Compliance Formatting Layer (D5), Notification Service (E1 - if implemented):** These new/migrated software components will likely be deployed on dedicated on-premise servers (virtual or physical) within the lab's network. OS to be determined (e.g., Linux, Windows Server).
    
- **Web UIs (e.g., for QA/Audit, admin tasks for BLDS components):** Deployed on a web server, co-located or separate from the backend services.
    
- **Existing Infrastructure:**
    
    - Network Drive, S3, Commercial Analysis Software, GUI-based analysis tools (to be updated).
        
- **New Hardware to be Provisioned:**
    
    - Tablets for data entry (accessing Network Drive).
        
    - Local Server(s) for migrated analysis scripts and new BLDS services.
        
- **Third-Party Managed Services:**
    
    - The ETL Service (C1) and the new central PostgreSQL database (C2) will be deployed and managed within the third-party provider's environment. Their query tools/interfaces and core export functionalities will also be part of this.
        
- Configuration management will be crucial for managing connection strings, file paths, API keys, interface specifications for third-party services, notification endpoints, etc., for all components.
    

## 8. Glossary

_(Key terms specific to this FDS, supplementing the SRS glossary. Could include names of specific in-house tools, equipment, or detailed process steps.)_

- **Automated Equipment Data Transfer Utility (A2):** A new software component (implemented in Rust) designed as part of BLDS to automatically monitor, copy, verify, and manage functional data files from equipment PCs to the network drive.
    
- **BLDS:** Biology Lab Data System (as defined in this FDS, referring to the components developed or migrated by the project and their integration with third-party services).
    
- **Data Preparation for ETL Module (B3.4):** A module within the Local Data Processing Server responsible for formatting and staging data for ingestion by the Third-Party Managed ETL Service.
    
- **Formatted Text Document Templates:** Document structures (existing or in development, e.g., Microsoft Word .docx, to be standardized by BLDS) used on tablets for manual data entry directly on the Network Drive.
    
- **In-house Analysis Scripts:** Existing scripts (e.g., Python, R) currently run on desktops, to be migrated to the Local Processing Server and updated.
    
- **Local Processing Server:** The on-premise server hosting migrated/updated analysis scripts, QA/Audit workflows, and new data preparation modules developed by the BLDS project.
    
- **Notification Service (E1):** A _potential future/stretch goal_ component for automatically notifying RAs/Scientists for subsequent experimental phases.
    
- **QA/Audit & Processing Error Handling Module (B3.3):** A BLDS component providing workflows and interfaces for quality assurance of manually entered data and resolution of errors/irregularities from data ingestion or ETL processes.
    
- **SEND:** Standard for Exchange of Nonclinical Data.
    
- **SEND Compliance Formatting Layer (D5):** A BLDS-developed component/process responsible for transforming data exported from the third-party system into SEND-compliant format.
    
- **Tablet Data Ingestion Service (A1):** A new software component designed as part of BLDS to monitor and process changes to standardized documents (modified by users on tablets on the network drive), preparing them for direct ETL ingestion or routing for local QA/processing.
    
- **Tablets:** Mobile devices (e.g., Windows, Android, iOS based, capable of running MS Word or equivalent and accessing network drives) to be provisioned as part of the BLDS project for direct digital data entry.
    
- **Third-Party Managed Database (C2):** The newly established central PostgreSQL database for BLDS, with its infrastructure and operational management provided by a third-party vendor.
    
- **Third-Party Managed ETL Service (C1):** The ETL service provided and managed by a third-party vendor, responsible for ingesting data prepared by BLDS (some via notification/monitoring of ready-to-ingest files) into the Third-Party Managed Database.
    
- _(Add other specific terms as needed during development)_
    

**End of Document**