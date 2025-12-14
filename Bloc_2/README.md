# 📦 Certification DE – Bloc 2
## Project - Amazon Review Analysis
## 🚀 Pipeline ETL
---

Pipeline ETL automatisé pour extraire, transformer et charger les données d'avis Amazon depuis PostgreSQL vers S3, Snowflake et MongoDB.

## Démarrage Rapide

### 1️⃣ Démarrage de l'infrastructure Docker

```bash
# 1. PostgreSQL (contient les données source - initialisation automatique)
docker-compose -f docker-compose.postgres.yml up -d

# 2. MongoDB (stocke les logs et données rejetées)
docker-compose -f docker-compose.mongodb.yml up -d

# 3. Airflow (orchestration du pipeline ETL)
docker-compose -f docker-compose.airflow.yml up -d
```

### 2️⃣ Configuration des credentials

**Créer le fichier `.env` à la racine:**
```bash
cp .env.example .env
# Éditer .env avec vos credentials AWS et Snowflake
```

**Variables obligatoires dans `.env`:**
```bash
# AWS S3
AWS_ACCESS_KEY_ID=votre_access_key
AWS_SECRET_ACCESS_KEY=votre_secret_key
AWS_S3_BUCKET=votre-bucket
AWS_REGION=eu-west-1

# Snowflake
SNOWFLAKE_USER=votre_user
SNOWFLAKE_PASSWORD=votre_password
SNOWFLAKE_ACCOUNT=votre_account
SNOWFLAKE_DATABASE=AMAZON_REVIEWS
SNOWFLAKE_SCHEMA=ANALYTICS
SNOWFLAKE_WAREHOUSE=COMPUTE_WH
SNOWFLAKE_ROLE=ACCOUNTADMIN

# PostgreSQL et MongoDB sont déjà configurés pour Docker
```

### 3️⃣ Déclencher le pipeline complet
```bash
docker exec airflow-webserver airflow dags trigger main_orchestrator
```

**Le pipeline exécutera automatiquement:**
1. ✅ Initialisation MongoDB (collections + indexes)
2. ✅ Initialisation Snowflake (database + schema + tables)
3. ✅ Extraction PostgreSQL → S3 (8 tables, anonymisation buyer_id)
4. ✅ Transformation et chargement → Snowflake + MongoDB

**Monitoring:**
- **Interface Airflow**: http://localhost:8080 (login: admin / password: admin)
- **MongoDB UI**: http://localhost:8081
- **Logs en temps réel**: `docker logs -f airflow-scheduler`

---

## Commandes Utiles

```bash
# Arrêter tous les services
docker-compose -f docker-compose.airflow.yml down
docker-compose -f docker-compose.mongodb.yml down
docker-compose -f docker-compose.postgres.yml down

# Redémarrer un service spécifique
docker-compose -f docker-compose.airflow.yml restart

# Réinitialiser PostgreSQL (supprime les données)
docker-compose -f docker-compose.postgres.yml down -v
docker-compose -f docker-compose.postgres.yml up -d
```

## Tests de Qualité

Le projet inclut une suite complète de tests de qualité des données :

```bash
cd src_code

# Exécuter les tests
python tests/test_data_quality.py

# Générer le rapport HTML
python scripts/generate_quality_report.py
```

**8 tests automatisés** :
- Connexion PostgreSQL
- Validation des ratings (1-5)
- Détection des doublons
- Champs obligatoires non-NULL
- Prix positifs
- Textes non-vides
- Cohérence des types
- Intégrité référentielle

Les rapports sont disponibles dans `src_code/reports/` (JSON + HTML).

## Technologies

- **Orchestration** : Apache Airflow 2.8.3 (Docker)
- **Source** : PostgreSQL 17 (Docker)
- **Data Lake** : AWS S3
- **Data Warehouse** : Snowflake
- **Logs & Rejets** : MongoDB 7.0 (Docker)
- **ETL** : Python 3.11+ avec pandas, boto3, snowflake-connector
- **Conteneurisation** : Docker & Docker Compose
