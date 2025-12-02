# Azure Monitor SCOM Managed Instance

## Conceptual design

The Azure Monitor SCOM Managed Instance along with its supporting infrastructure will be deployed in its own landing zone in Sweden Central. 
The landing zone will be a standard internal landing zone with peering to our hub.

```mermaid
graph LR
    SCOM(SCOM MI)
    AzFW(Azure Firewall)
    SQL(SQL Managed instance)
    SCOMGW2[[SCOM Gateway]]
    VM1(VMs)
    VM2(VMs)
    FW(On-prem firewall)
    SCOMGW1[[SCOM Gateway]]

    subgraph "SCOM landing zone"
        SCOM
        SQL
        SCOMGW2
    end

    subgraph "Hub"
        AzFW
    end

    subgraph "Other internal landing zone"
        VM1
    end

    subgraph "On-prem"
        VM2
        FW
        SCOMGW1
    end

    VM1 <--> AzFW
    VM2 <--> SCOMGW1
    SCOMGW1 <--> FW
    FW <--> AzFW
    AzFW <--> SCOMGW2
    SCOMGW2 --> SCOM
    SCOM --> SQL

```

## Logical design

![Logical design](Logical.png)