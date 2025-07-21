# PR and Ephemeral Build

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GH as SCM
    participant GA as Automation
    participant Nexus as Artifact Repository
    participant OSD as Nonprod
    participant App as Application
    participant Tests as Automated Tests

    Dev->>GH: Opens Pull Request <br/>against main branch
    GH->>GA: Triggers CI Pipeline
    
    Note over GA: Snapshot build Process
    GA->>GA: Checkout code from PR
    GA->>GA: Build application
    GA->>GA: Run unit tests
    GA->>GA: Create snapshot build
    
    GA->>Nexus: Upload snapshot artifact <br/>to nonprod repo
    Nexus-->>GA: Confirm upload
    
    Note over GA,OSD: Snapshot Deployment Process
    GA->>OSD: Create / provision ephemeral environment
    OSD-->>GA: Environment created / provisioned
    
    GA->>OSD: Deploy application to ephemeral environment
    OSD->>App: Start application
    App-->>OSD: Application ready
    OSD-->>GA: Deployment successful
    
    Note over GA,Tests: Testing Process
    GA->>Tests: Run automated tests
    Tests->>App: Execute test suite
    App-->>Tests: Test responses
    Tests-->>GA: Test results
    GA-->>GH: Post test results to PR
    
    Note over GA,GH: Notification Process
    GA->>GA: Generate application URL
    GA->>GH: Post comment on PR
    GH->>Dev: Notify with application URL
    
    Note over Dev,OSD: Developer can now review the deployed ephemeral application
    Dev->>GH: Merge pull request
    GH->>GA: Triggers CI Pipeline
    
    Note over GA: Ephemeral cleanup process
    GA->>OSD: Delete / deprovision ephemeral environment
    GA->>Nexus: Delete snapshot build

```

# Promotion - Nonprod

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GH as SCM
    participant GA as Automation
    participant Nexus as Artifact Repository
    participant OSD as Nonprod
    participant App as Application
    participant TL as Approver

    Dev->>GH: Merge pull request
    GH->>GA: Triggers CI Pipeline

    Note over GA: Release Candidate build Process
    GA->>GA: Checkout code from main
    GA->>GA: Build application
    GA->>GA: Run unit tests
    GA->>GA: Create RC build
    
    Note over GA,OSD: DEV Deployment Process (Automated)
    GA->>OSD: Deploy application <br/>to DEV environment
    OSD->>App: Start application
    App-->>OSD: Application ready
    OSD-->>GA: Deployment successful
    
    Note over Dev,OSD: Developer can now review the deployed application in DEV
    Dev->>GA: Request Promotion to UAT
    GA->>TL: Requests approval
    TL->>GA: Approves promotion request

    GA->>OSD: Deploy application to UAT environment 
    OSD->>App: Start application
    App-->>OSD: Application ready
    OSD-->>GA: Deployment successful

    Note over Dev,OSD: Developer can now review the deployed application in UAT

```
# Promotion - Prod

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GA as Automation
    participant Nexus as Repository
    participant App as Application
    participant TL as Approver
    participant OSP as Prod

    Dev->>GA: Request Promotion to Prod
    GA->>TL: Requests approval
    TL->>GA: Approves promotion request

    GA->>Nexus: Upload RC to prod repository

    GA->>OSP: Deploy application to Prod environment
    OSP->>App: Start application
    App-->>OSP: Application ready
    OSP-->>GA: Deployment successful

```

