```mermaid
flowchart TD
    A[Utilisateur] -->|1. Requête HTTP| B[DNS]
    B -->|2. Résolution: 8.8.8.8| C[Serveur\nUbuntu 22.04]

    subgraph Serveur["Serveur LAMP (8.8.8.8)"]
        C --> D[Nginx\nPort 80]
        D -->|Fichiers statiques| E[/HTML/CSS/JS/]
        D -->|Requête dynamique| F[PHP-FPM]
        F -->|Requêtes SQL| G[(MySQL\nPort 3306)]
        G -->|Données| F
        F -->|Réponse PHP| D
    end

    D -->|3. Réponse HTTP| A

    style A fill:#f9f,stroke:#333
    style B fill:#bbf,stroke:#333
    style C fill:#f96,stroke:#333
    style D fill:#6f6,stroke:#333
    style F fill:#6f6,stroke:#333
    style G fill:#66f,stroke:#333
    style E fill:#ff9,stroke:#333
