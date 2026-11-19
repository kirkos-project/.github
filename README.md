# kirkos

**kirkos** is a proof-of-concept detection pipeline utilising [Vector](https://vector.dev/) for routing logs from different sources to detection rule engines. 

## Detection Pipeline

Below is a high-level view of the pipeline. Clicking on implemented nodes will direct you to the relevant repo. 

```mermaid
---
config:
  theme: 'neo'
  look: 'handDrawn'
  themeVariables:
    fontFamily: '"Chalkboard SE", "Marker Felt", "Comic Sans MS", cursive'
---
flowchart LR;
  subgraph Ingestion["Ingestion (Not Implemented)"]
    ingestAWS["AWS Log Ingester"]
    ingestAzure["Azure Log Ingester"]
    ingestWin["Windows Log Ingester"]
  end
  subgraph IntelGen["Intel Preprocessing"]
    ipIntel["IP Intel Unifier"]
  end
  subgraph Enrichment
    subgraph AWSEnrichment["AWS Enrichers"]
      enrichAWSBase["Base AWS Enricher"]
    end
    subgraph AzureEnrichment["Azure Enrichers"]
      enrichAzure["Azure-specific Enrichers 
      (To Be Implemented)"]
    end
    subgraph WinEnrichment["Windows Enrichers"]
      enrichWin["Windows-specific Enrichers 
      (To Be Implemented)"]
    end
    subgraph CommonEnrichment["Common Enrichers"]
      enrichIP["IP Intel Enricher"]
    end
  end
  subgraph Detection
    detSigma["Sigma Detection Rules"]
  end
  
  ingestAWS-->enrichAWSBase;
  ingestAzure-->enrichAzure;
  ingestWin-->enrichWin;
  enrichAWSBase-->enrichIP;
  enrichAzure-->enrichIP;
  enrichWin-->enrichIP;
  ipIntel-.->enrichIP;
  enrichIP-->Detection;
  
  %% Define a theme/style for clickable nodes.
  classDef clickable fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0369a1,font-weight:bold;

  class enrichAWSBase,ipIntel,enrichIP,detSigma clickable;
  
  %% Syntax: click NodeID "URL" "Optional Tooltip" _target
  click enrichAWSBase "https://codeberg.org/kirkos-project/kirkos-aws-enricher-base" _blank
  click ipIntel "https://codeberg.org/kirkos-project/kirkos-unified-ip-threat-db" _blank
  click enrichIP "https://codeberg.org/kirkos-project/kirkos-common-enricher-ip" _blank
  click detSigma "https://codeberg.org/kirkos-project/kirkos-sigma-rules" _blank
```