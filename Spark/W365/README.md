# Windows 365 PoC

This design is for an implementation of Windows 365 that can be set up and used as a proof of concept.

## Conceptual design

The design is based on setting up Windows 365 with Cloud PCs that will be connected a vNet in Azure.
From the the vNet, the Cloud PCs will be able to access other resources in Azure and they will have a connection to the internal network.
The following diagram shows the setup at a high level:

```mermaid	
flowchart LR
    User(User)
    W365(Windows 365 control plane)
    CloudPC(Cloud PC)
    ServiceA(Service A)
    ServiceB(Service B)
    GSA([Global Secure Access])
    AzFW([Azure Firewall])

    subgraph Anywhere
        User
    end

    subgraph Azure
        subgraph W365 landing zone
            subgraph vNet
                CloudPC
            end
        end
        subgraph Other landing zone
            ServiceB
        end
        AzFW
    end

    subgraph On-prem
        ServiceA
    end

    User --> W365
    W365 --> CloudPC
    AzFW --> ServiceA
    CloudPC --> AzFW --> ServiceB
```	

## Logical design

The logical design is based on the following components:

![Logical design](Logical-W365.png)
