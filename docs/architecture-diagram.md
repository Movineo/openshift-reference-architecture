graph TD
    %% Styling Definitions
    classDef infra fill:#f4f4f4,stroke:#333,stroke-width:2px,color:#000;
    classDef openshift fill:#EE0000,stroke:#fff,stroke-width:2px,color:#fff;
    classDef automation fill:#0066cc,stroke:#fff,stroke-width:2px,color:#fff;
    classDef registry fill:#003366,stroke:#fff,stroke-width:2px,color:#fff;
    classDef observability fill:#ff9900,stroke:#fff,stroke-width:2px,color:#000;

    %% 1. Automation & CI/CD Layer
    subgraph Automation_Layer ["Automation & CI/CD Pipeline"]
        Ansible["Ansible Playbooks<br/>(Cluster Deployment)"]:::automation
        Tekton["Tekton Pipelines<br/>(App Build & Deploy)"]:::automation
    end

    %% 2. Registry Layer
    subgraph Registry_Layer ["Container Registry"]
        Quay["Red Hat Quay<br/>(Image Management)"]:::registry
    end

    %% 3. OpenShift Cluster Layer
    subgraph OpenShift_Cluster ["Red Hat OpenShift Container Platform"]
        ControlPlane["Control Plane (Master Nodes)<br/>API, Scheduler, etcd"]:::openshift
        
        subgraph Worker_Layer ["Worker Nodes (Application Workloads)"]
            AppPods["Application Pods"]
            Ingress["Ingress / Router Layer"]
        end
    end

    %% 4. Observability Layer
    subgraph Observability_Layer ["Monitoring & Logging Stack"]
        Prometheus["Prometheus<br/>(Metrics Collection)"]:::observability
        Loki["Loki<br/>(Log Aggregation)"]:::observability
        Grafana["Grafana<br/>(Visualization)"]:::observability
    end

    %% 5. Base Infrastructure Layer
    subgraph Infra_Layer ["Infrastructure Layer"]
        RHEL["RHEL / Rocky Linux 9 OS<br/>(Hypervisor / VMs)"]:::infra
    end

    %% Relationships and Traffic Flow
    
    %% Ansible provisioning flow
    Ansible -->|Provisions & Configures OS| RHEL
    Ansible -->|Bootstraps Cluster| ControlPlane
    
    %% App Deployment Flow
    Tekton -->|Builds & Pushes Images| Quay
    Tekton -->|Triggers Deployment| ControlPlane
    ControlPlane -->|Orchestrates Pods| Worker_Layer
    Quay -.->|Pulls Container Images| Worker_Layer
    
    %% Base Layer mapping
    ControlPlane ---|Runs on| RHEL
    Worker_Layer ---|Runs on| RHEL
    
    %% Observability Flow
    Prometheus -->|Scrapes Node & Pod Metrics| Worker_Layer
    Loki -->|Collects Container Logs| Worker_Layer
    Prometheus -.->|Feeds Data| Grafana
    Loki -.->|Feeds Data| Grafana
