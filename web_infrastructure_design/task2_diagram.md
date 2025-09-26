```mermaid
flowchart TD
    %% --- Styles ---
    classDef user fill:#f9f,stroke:#333
    classDef lb fill:#bbf,stroke:#333
    classDef server fill:#f96,stroke:#333
    classDef webserver fill:#6f6,stroke:#333
    classDef app fill:#6f6,stroke:#333
    classDef db fill:#66f,stroke:#333
    classDef files fill:#ff9,stroke:#333
    classDef firewall fill:#f96,stroke:#333,stroke-dash:5
    classDef ssl fill:#6f9,stroke:#333
    classDef monitor fill:#9f9,stroke:#333

    %% --- Elements ---
    User[User]:::user -->|1. HTTPS Request| FW1[Firewall]:::firewall
    FW1 --> LB[Load Balancer\nHAProxy\nSSL Termination]:::lb
    LB --> FW2[Firewall]:::firewall
    FW2 -->|2. Decrypted Traffic| S1[Server 1\nUbuntu 22.04]:::server
    FW2 -->|2. Decrypted Traffic| S2[Server 2\nUbuntu 22.04]:::server

    %% --- Server 1 ---
    subgraph Server1["Server 1 (8.8.8.1)"]
        S1 --> W1[Nginx\nPort 443]:::webserver
        W1 -->|Static Files| F1[/HTML/CSS/JS/]:::files
        W1 -->|Dynamic Request| A1[PHP-FPM]:::app
        A1 -->|SQL Queries| DB1[(MySQL Primary\nPort 3306)]:::db
        DB1 -->|Data| A1
        A1 -->|PHP Response| W1
        W1 --> M1[Monitoring\nAgent]:::monitor
    end

    %% --- Server 2 ---
    subgraph Server2["Server 2 (8.8.8.2)"]
        S2 --> W2[Nginx\nPort 443]:::webserver
        W2 -->|Static Files| F2[/HTML/CSS/JS/]:::files
        W2 -->|Dynamic Request| A2[PHP-FPM]:::app
        A2 -->|SQL Queries| DB2[(MySQL Replica\nPort 3306)]:::db
        DB2 -->|Data| A2
        A2 -->|PHP Response| W2
        W2 --> M2[Monitoring\nAgent]:::monitor
    end

    %% --- Database Replication ---
    DB1 -->|3. Asynchronous\nReplication| FW3[Firewall]:::firewall
    FW3 --> DB2

    %% --- SSL Certificate ---
    LB -->|4. SSL Certificate\nwww.foobar.com| Cert[Let's Encrypt\nWildcard]:::ssl

    %% --- Monitoring ---
    M1 -->|Metrics| Monitor[SumoLogic\nDashboard]:::monitor
    M2 -->|Metrics| Monitor
    LB -->|Stats| Monitor

    %% --- Response Path ---
    W1 -->|5. HTTP Response| FW2
    W2 -->|5. HTTP Response| FW2
    FW2 --> FW1
    FW1 -->|6. Encrypted Response| User
