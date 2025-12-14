# Pipeline ETL - Documentation Technique

Pipeline d'extraction, transformation et chargement des avis Amazon : **PostgreSQL → S3 → Snowflake + MongoDB**

## Prérequis

1. **Python 3.11+** avec les dépendances installées
2. **Docker** en cours d'exécution
3. **Bases de données démarrées** :
   ```bash
   # Depuis la racine du projet
   docker-compose -f docker-compose.postgres.yml up -d        # PostgreSQL
   docker-compose -f docker-compose.mongodb.yml up -d         # MongoDB (depuis src_code/)
   ```

## Installation

```bash
# Installer les dépendances
pip install -r requirements.txt

# Configurer les credentials
cp .env.example .env
# Éditer .env avec vos credentials AWS et Snowflake
```

## Configuration

### Fichier `.env`

```bash
# PostgreSQL (déjà configuré pour Docker local)
POSTGRES_CONNECTION_STRING=postgresql://admin:admin123@localhost:5433/amazon_db

# AWS S3 (à remplir)
AWS_ACCESS_KEY_ID=votre_access_key
AWS_SECRET_ACCESS_KEY=votre_secret_key
AWS_S3_BUCKET=votre_bucket
AWS_REGION=eu-west-1

# Snowflake (à remplir)
SNOWFLAKE_ACCOUNT=votre_account
SNOWFLAKE_USER=votre_user
SNOWFLAKE_PASSWORD=votre_password
SNOWFLAKE_WAREHOUSE=COMPUTE_WH
SNOWFLAKE_DATABASE=AMAZON_REVIEWS
SNOWFLAKE_SCHEMA=STAGING
SNOWFLAKE_SCHEMA_ANALYTICS=ANALYTICS
SNOWFLAKE_ROLE=ACCOUNTADMIN

# MongoDB (déjà configuré pour Docker local)
MONGODB_CONNECTION_STRING=mongodb://admin:changeme@localhost:27017/amazon_reviews?authSource=admin
```

## Architecture du Pipeline

```
┌─────────────────┐
│  PostgreSQL     │  
│  (localhost)    │ 
└────────┬────────┘   
         │              
         ↓
    Extract to S3
         │
         ↓
┌─────────────────┐
│    AWS S3       │  
│  (Data Lake)    │  
└────────┬────────┘
         │
         ↓
  Transform and load
         │
         ├──────────────────┐
         ↓                  ↓
┌─────────────────┐  ┌─────────────────┐
│   Snowflake     │  │    MongoDB      │
│ (Data Warehouse)│  │  (Logs & Meta)  │
│                 │  │                 │
│ Table: reviews  │  │ Collection:     │
│ 111K rows       │  │ - pipeline_logs │
│ Données nettoyées│ │ - metadata      │
└─────────────────┘  └─────────────────┘
```

## Configuration Avancée

### Fichier `config/config.yaml`

```yaml
tables:
  - product          
  - category         
  - review            
  - product_reviews  
  - review_images     
  - orders            

s3:
  raw_prefix: "raw/"

snowflake:
  table_name: "reviews"
  rejected_table: "rejected_reviews"
```


## Logs

Les logs sont automatiquement sauvegardés :
- **MongoDB** : Collection `pipeline_logs`
- **Console** : Output en temps réel avec timestamps
