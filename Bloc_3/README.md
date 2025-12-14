# 📦 Certification DE – Bloc 3  
## Project - Amazon Review Analysis
## 🚀 Mise en Production & Maintenance  
---


## 🚀 Présentation du projet

Pour rappel le projet Amazon Review Analysis a pour but de développer une solution automatisée pour classer les avis des produits et identifier leur pertinence.

---

## 🎯 Objectifs

- Déployer l’architecture complète.
- Garantir la **fiabilité, la sécurité et la performance** du système
- Fournir une **documentation complète** pour les utilisateurs et les équipes d’exploitation
- Mettre en place des **procédures de maintenance et de gestion des incidents**
- Préparer le système aux **évolutions futures et aux contraintes réglementaires**

---


## 🏗️ Vue d’ensemble de l’architecture

L’architecture de production comprend :

- **PostgreSQL** – Base de données Source configurée.
- **S3** - Data Lake pour stockage des données brutes.

- **Snowflake** - Datawarehouse pour stockage propres.
- **MongoDB** - Base de données NoSQl pour le stockage des données rejetées et des logs.
- **Pipeline d’analyse des avis** – Catégorisation et score de pertinence.
- **API** – Exposition des avis catégorisés
- **Frontend e-commerce mocké** – Simulation des interactions utilisateurs (Business Analyst)
- **Monitoring & alerting** – Supervision du système
- **Sécurité & RBAC** – Gestion des rôles et des accès


---

## 🛍️ Intégration du frontend

Un frontend simplifié permet de simuler un usage réel :

- Consultation du catalogue produits
- Recherche des avis pertinents par produit
- Affichage des **avis clients les plus pertinents**

---

## 📋 Livrables du bloc

Le bloc s’articule autour de **quatre livrables principaux**, disponibles sous forme de documents détaillés :

---

### 1️⃣ Solution e-commerce  

Application fonctionnelle
`/src/amazon-mockup-e-commerce/`

---

### 2️⃣ Compte Rendu de Mise en Production
*(Production Deployment Report)*

📎 Document :  
`/docs/pdf_docs/Bloc-3-step1-Production-Deployment-Report-20251210-Dyhia-TOUAHRI.pdf`

---
### 3️⃣ Dossier d’Accompagnement Utilisateur
*(User Support Documentation)*

📎 Document :  
`/docs/pdf_docs/Bloc-3-step2-User-Support-Documentation-20251211-Dyhia-TOUAHRI.pdf`

---

###  4️⃣ Dossier de Maintenance 
*(Maintenance Documentation)*

📎 Document :  
`/docs/pdf_docs/Bloc-3-Step3-Maintenance Documentation-20251212-Dyhia-TOUAHRI.pdf`

---