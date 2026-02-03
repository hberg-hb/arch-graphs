```mermaid

graph TD
    User[User (Browser)] -->|1. Submit Code| NextJS[Next.js API Route /api/submit]
    
    subgraph "The Web Layer (Next.js)"
        NextJS -->|2. Auth & Validate| Postgres[(PostgreSQL)]
        NextJS -->|3. Push Job| RabbitMQ[RabbitMQ Queue]
        NextJS -->|7. Poll Status| Redis[(Redis Cache)]
    end

    subgraph "The Background Processing Layer"
        RabbitMQ -->|4. Pull Job| Worker[Node.js Worker Service]
        Worker -->|5. Fetch Test Cases| S3[AWS S3 / MinIO]
        
        subgraph "The Sandbox"
            Worker -->|6. Execute| Docker[Docker Container]
        end
        
        Worker -->|8. Update Status| Redis
        Worker -->|9. Save Result| Postgres
    end

```