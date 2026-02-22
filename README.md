# Application de Vote - Projet Conteneurisé

## Prérequis

- Docker (version 20.10+)
- Docker Compose (version 2.0+)

## Installation

```bash
docker compose up -d
```

## Utilisation

- Interface de vote :
- Interface de résultats : http://localhost:8888

## Arrêt

```bash
docker compose down
```

## Architecture

L'application est composée de 5 conteneurs :

- **vote** (Python/Flask) : Interface de vote sur le port 8080
- **redis** : File d'attente pour les votes
- **worker** (.NET) : Traitement des votes entre Redis et PostgreSQL
- **db** (PostgreSQL) : Stockage des votes
- **result** (Node.js) : Affichage des résultats sur le port 8888

## Réseaux

Deux réseaux isolés :
- `vote-network` : vote ↔ redis ↔ worker
- `result-network` : result ↔ db ↔ worker

## Volumes

- `db-data` : Persistance des données PostgreSQL

## Modifications du code

### vote/app.py
Ligne 19 : Ajout de `REDIS_HOST` via variable d'environnement pour permettre la connexion à Redis dans Docker.

### worker/Program.cs
Lignes 18-23 : Ajout de variables d'environnement (`POSTGRES_HOST`, `REDIS_HOST`, `POSTGRES_USER`, `POSTGRES_PASSWORD`) pour la configuration dynamique des connexions.

### result/server.js
Lignes 14-18 : Ajout de variables d'environnement pour la configuration PostgreSQL (`POSTGRES_HOST`, `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`).

Toutes les modifications conservent les valeurs par défaut (localhost) pour la rétrocompatibilité.
 

 -------------------------------------------------------------

 Lancement du projet : 



 But: Démarrer le projet avec uniquement Docker installé (sans images ni conteneurs préexistants).

Prérequis: Docker installé et en cours d'exécution. Ports 8080 et 8888 disponibles.

Étapes (minimum pour lancer)

Copier/coller le dépôt et ouvrir le dossier racine du projet (contenant docker-compose.yml).
Construire et démarrer tous les services:

docker compose up --build -d

Vérifier les logs si besoin:

docker compose logs -f

Accéder aux services depuis un navigateur:
Vote: 

Résultats: http://localhost:8888

Arrêter et supprimer les conteneurs

docker compose down

(si vous voulez aussi supprimer les volumes de données PostgreSQL):

docker compose down -v

Remarques utiles
La première exécution télécharge les images (Redis, Postgres) et construit les images locales (vote, worker, result).
Les variables d'environnement nécessaires (Postgres/Redis) sont déjà définies dans docker-compose.yml.
Si un service met du temps à démarrer (ex: DB), attendre quelques secondes puis vérifier docker compose ps et docker compose logs.
En cas de conflit de ports, modifier les mappings de ports dans docker-compose.yml.
