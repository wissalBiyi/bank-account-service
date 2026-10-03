# Rapport de Projet : Bank Account Service (Systèmes Distribués et DevOps)

## Introduction
Dans le cadre du module **Systèmes Distribués et DevOps**, ce projet a pour objectif de concevoir et de développer un micro-service orienté métier, baptisé `bank-account-service`. Ce service met en œuvre les standards modernes de l'architecture logicielle en s'appuyant sur l'écosystème **Spring**. L'application permet la gestion complète de comptes bancaires (création, consultation par ID, liste globale) tout en intégrant des mécanismes robustes de persistance des données, d'exposition d'API REST documentées, ainsi que des technologies modernes comme Spring Data REST et GraphQL. Ce rapport présente la démarche adoptée ainsi que les différentes phases de test et de validation des endpoints à l'aide d'outils professionnels tels que Postman, Swagger UI et les interfaces de consultation HAL.

---

## 1. Architecture et Technologies Utilisées
* **Java / Spring Boot** : Framework principal pour le développement du micro-service.
* **Spring Data JPA / Hibernate** : Gestion de la persistance des données et de l'ORM.
* **H2 Database** : Base de données en mémoire pour le développement et les tests.
* **Springdoc OpenAPI (Swagger)** : Documentation interactive des API REST.
* **Postman** : Test des endpoints HTTP et des requêtes GraphQL.

---

## 2. Tests des API REST (Postman)

### 2.1. Récupération de la liste des comptes (`GET`)
Cette requête permet d'afficher l'ensemble des comptes bancaires enregistrés dans le système.
![Liste des comptes Postman](./images/img1.jpeg)

### 2.2. Recherche d'un compte par ID (`GET /bankAccounts/{id}`)
Permet de récupérer les détails d'un compte spécifique à partir de son identifiant unique (UUID).
![Recherche par ID Postman](./images/img2.png)

---

## 3. Documentation et Tests via Swagger UI

### 3.1. Test de la recherche par ID sur Swagger
Interface Swagger permettant de tester l'endpoint de recherche par ID en renseignant le paramètre requis.
![Swagger Get by ID](./images/img3.png)

### 3.2. Création d'un compte bancaire (`POST`)
Envoi d'un objet JSON contenant les informations du compte (`balance`, `currency`, `type`) pour l'enregistrer via l'API.
![Swagger Post Account](./images/img4.png)
*(Autre exemple de requête POST validée :)*
![Swagger Post Ex 2](./images/img5.png)

---

## 4. Consultation via le Navigateur (Spring Data REST & HATEOAS)
Affichage brut des données sous format HAL/JSON avec les liens hypermédia associés (`_links`).
* **Liste globale :**
  ![Browser List](./images/img6.png)
* **Détail d'un compte :**
  ![Browser Detail](./images/img7.png)

---

## 5. Tests et Manipulation via GraphQL

En complément des API REST, le micro-service intègre une interface **GraphQL** accessible sur l'endpoint `/graphql` permettant d'interroger et de manipuler les données avec précision (en évitant l'over-fetching ou sous-fetching).

### 5.1. Consultation de la liste des comptes (`accountsList`)
Récupération de la liste de tous les comptes bancaires via une requête GraphQL.
![GraphQL Accounts List](./images/img8.png)

### 5.2. Consultation d'un compte par ID (`bankAccountById`)
Recherche d'un compte spécifique en fournissant son identifiant.
![GraphQL Account By ID](./images/img9.png)

### 5.3. Gestion des erreurs en cas d'ID inexistant
Si l'identifiant recherché n'existe pas dans la base de données, l'API GraphQL renvoie un message d'erreur explicite tout en informant que le compte est `null`.
![GraphQL Error Handling](./images/img10.png)

### 5.4. Ajout d'un compte (`addAccount` / Mutation)
Création d'un nouveau compte bancaire en passant les paramètres directement dans la mutation.
![GraphQL Add Account 1](./images/img11.png)
*(Utilisation des variables GraphQL pour l'ajout :)*
![GraphQL Add Account with Variables](./images/img12.png)

### 5.5. Modification d'un compte (`updateAccount` / Mutation)
Mise à jour des informations d'un compte bancaire existant (type, solde, devise) à l'aide de son identifiant et de variables GraphQL.
![GraphQL Update Account](./images/img13.png)

### 5.6. Suppression d'un compte (`deleteAccount` / Mutation)
Suppression d'un compte existant à l'aide de son ID, renvoyant une confirmation booléenne (`true`).
![GraphQL Delete Account](./images/img14.png)

### 5.7. Gestion des Clients (`Customers`) et Relation avec les Comptes
Le modèle de données intègre également la gestion des clients (`Customers`).
* **Liste des clients en base de données (H2 Console) :**
  ![H2 Customers Table](./images/img15.png)
* **Liste globale des comptes avec détails de la base (H2 Console) :**
  ![H2 Bank Accounts Table](./images/img16.png)
* **Association Client-Comptes via GraphQL :** Affichage des clients et de leurs comptes associés grâce aux requêtes relationnelles GraphQL (`customer` avec ses `bankAccounts`).
  ![GraphQL Customers and Accounts](./images/img17.png)
* **Consultation globale au format JSON / API REST :**
  ![API JSON Result](images/img18.png)

---

## 6. Conclusion
Ce projet m'a permis de valider la mise en place de micro-services Spring Boot, l'exposition d'API REST documentées via Swagger, l'utilisation de Postman pour les tests, l'intégration d'un serveur GraphQL pour des requêtes flexibles, ainsi que la gestion de la persistance des données et des relations complexes (Clients/Comptes) dans un contexte de systèmes distribués.
