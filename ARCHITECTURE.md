# Architecture Technique de la Solution

Voici le schéma de notre infrastructure Cloud :

```mermaid
graph TD
%% -- STYLES --
classDef mobile fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
classDef server fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
classDef db fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,stroke-dasharray: 5 5;
classDef ext fill:#eceff1,stroke:#546e7a,stroke-width:1px;

%% -- ACTEURS --
Student((Étudiant))
StoreMgr((Magasin))

%% -- FRONTEND --
subgraph Clients ["Interface Utilisateur (Web App)"]
    WebApp[("Application Web<br>(HTML/JS)")]:::mobile
end

%% -- BACKEND --
subgraph Backend ["Serveur Backend"]
    APIServer["API REST<br>(Node.js Express)"]:::server
    
    %% Logique interne simplifiée
    Logic1[routes/paniers]:::server
    Logic2[routes/auth]:::server
end

%% -- DATABASE --
subgraph Data ["Données"]
    SQLite[("SQLite<br>(Fichier local .db)")]:::db
end

%% -- FLUX --
Student -->|Commande| WebApp
StoreMgr -->|Ajoute Panier| WebApp

WebApp -->|Requêtes HTTP JSON| APIServer

APIServer --- Logic1
APIServer --- Logic2

APIServer -->|Lecture/Écriture| SQLite

%% -- SERVICES EXTERNES --
Stripe[("Simulation Paiement")]:::ext
APIServer -.-> Stripe
