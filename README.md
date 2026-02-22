# Application de Vote - Projet Conteneurisé

## Objectif

Déployer une application distribuée complète à l’aide de Docker et docker-compose, sans dépendance locale (Python, Node, .NET ou PostgreSQL installés sur la machine).

---

## Architecture

L’application est composée de 5 conteneurs :

- **vote** (Python / Flask)  
  Interface de vote – port 8080  

- **redis**  
  File d’attente des votes  

- **worker** (.NET)  
  Traitement des votes entre Redis et PostgreSQL  

- **db** (PostgreSQL)  
  Stockage des votes  

- **result** (Node.js)  
  Affichage des résultats – port 8888  

---

## Réseaux Docker

Deux réseaux isolés :

- `vote-network` : vote ↔ redis ↔ worker  
- `result-network` : result ↔ db ↔ worker  

---

## Volume

- `db-data` : persistance des données PostgreSQL  

---

## Prérequis

- Docker (20.10+)
- Docker Compose (2.0+)

---

## Lancement du projet

Depuis le dossier contenant `docker-compose.yml` :

```bash
docker compose up --build -d
