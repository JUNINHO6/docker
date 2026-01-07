Documentation Technique : Deploiement Multi-Services Docker

Ce document fournit les instructions necessaires pour le deploiement et la gestion de l'infrastructure conteneurisee de l'application (Frontend, API, Base de donnees).

1. Prerequis du systeme

Pour assurer le bon fonctionnement de l'orchestration, les elements suivants doivent etre installes :
- Docker Engine : version 20.10.0 ou superieure
- Docker Compose : version 2.0.0 ou superieure
- Systemes d'exploitation supportes : Windows (via Docker Desktop), macOS ou Linux

2. Procedure de deploiement

Le lancement des services s'effectue via une commande unique a la racine du projet. Cette commande assure la construction des images et le demarrage des conteneurs en mode detache.

```bash
docker compose up --build -d
```

Acces aux services :
- Interface Frontend : http://localhost:8080
- Point d'entree API : http://localhost:5000/api
- Etat de sante de l'API : http://localhost:5000/api/health

3. Description des services et configuration

L'architecture repose sur trois services distincts communiquant au sein d'un reseau isole.

Description des services :
- frontend : Application React servie par Nginx. Port hote 8080.
- backend : API Express assurant la logique metier. Port hote 5000.
- db : Instance PostgreSQL 15 pour le stockage des donnees. Port interne 5432.

Variables d'environnement :
Les parametres de connexion a la base de donnees sont definis dans le fichier .env (DB_USER, DB_PASSWORD, DB_NAME).

4. Choix techniques et architecture

L'infrastructure a ete concue selon les principes suivants :

- Isolation des services : Chaque composant s'execute dans son propre conteneur, garantissant une meilleure stabilite et facilite de mise a jour.
- Optimisation des images : Le service frontend utilise un multi-stage build pour minimiser l'empreinte disque en production.
- Persistance des donnees : L'utilisation d'un volume nomme (postgres_data) assure que les donnees de la base ne sont pas liees au cycle de vie du conteneur.
- Securite reseau : Les echanges entre l'API et la base de donnees s'effectuent sur un reseau prive virtuel, non expose directement sur la machine hote.

5. Arret des services

Pour stopper l'infrastructure et liberer les ressources :

```bash
docker compose down -v
```
