# System Architecture

```mermaid
graph TD
    A[Robot Logs] -->|JSON Logs| B[Promtail]
    B -->|Parse & Transform| C[Loki]
    C -->|Query| D[Grafana]
    D -->|Visualize| E[Dashboard]
    
    subgraph Log Collection
    A
    B
    end
    
    subgraph Log Storage
    C
    end
    
    subgraph Visualization
    D
    E
    end
    
    F[CI/CD Pipeline] -->|Deploy| B
    F -->|Deploy| C
    F -->|Deploy| D
