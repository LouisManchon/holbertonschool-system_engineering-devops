```mermaid
flowchart TD
    %% --- Styles cohérents avec Task 0 ---
    classDef user fill:#f9f,stroke:#333
    classDef lb fill:#bbf,stroke:#333
    classDef server fill:#f96,stroke:#333
    classDef webserver fill:#6f6,stroke:#333
    classDef app fill:#6f6,stroke:#333
    classDef db fill:#66f,stroke:#333
    classDef files fill:#ff9,stroke:#333

    %% --- Éléments ---
    User[Utilisateur]:::user -->|1. Requête HTTP| LB[Load Balancer\nHAProxy]:::lb
    LB -->|2. Distribution\nRound Robin| S1[Serveur 1\nUbuntu 22.04]:::server
    LB -->|2. Distribution\nRound Robin| S2[Serveur 2\nUbuntu 22.04]:::server

    %% --- Serveur 1 ---
    subgraph S1_G["Serveur 1 (8.8.8.1)"]
        S1 --> W1[Nginx\nPort 80]:::webserver
        W1 -->|Fichiers statiques| F1[/HTML/CSS/JS/]:::files
        W1 -->|Requête dynamique| A1[PHP-FPM]:::app
        A1 -->|Requêtes SQL| DB1[(MySQL Primary\nPort 3306)]:::db
        DB1 -->|Données| A1
        A1 -->|Réponse PHP| W1
    end

    %% --- Serveur 2 ---
    subgraph S2_G["Serveur 2 (8.8.8.2)"]
        S2 --> W2[Nginx\nPort 80]:::webserver
        W2 -->|Fichiers statiques| F2[/HTML/CSS/JS/]:::files
        W2 -->|Requête dynamique| A2[PHP-FPM]:::app
        A2 -->|Requêtes SQL| DB2[(MySQL Replica\nPort 3306)]:::db
        DB2 -->|Données| A2
        A2 -->|Réponse PHP| W2
    end

    %% --- Réplication ---
    DB1 -->|3. Réplication\nAsynchrone| DB2

    %% --- Réponse finale ---
    W1 -->|4. Réponse HTTP| User
    W2 -->|4. Réponse HTTP| User
