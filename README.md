# Projet Microservices – Spring Boot, Eureka, Gateway, Client & Car

Ce projet présente une architecture microservices simple basée sur Spring Cloud, comprenant :

- **Eureka Server** – registre des services  
- **API Gateway** – point d’entrée unique  
- **SERVICE-CLIENT** – gestion des clients  
- **SERVICE-CAR** – gestion des voitures + récupération du propriétaire via SERVICE-CLIENT  

---

## 1. Prérequis

- Java 17  
- Maven  
- MySQL (port 3306)  
- Un IDE (IntelliJ, Eclipse, VS Code…)  
- Spring Initializr  

---

## 2. Architecture générale

| Service        | Rôle                              | Port     | Base            |
|----------------|-----------------------------------|----------|-----------------|
| Eureka Server  | Registry                          | **8761** | -               |
| SERVICE-CLIENT | Gestion des clients               | **8081** | clientservicedb |
| SERVICE-CAR    | Gestion voitures + client associé | **8082** | carservicedb    |
| Gateway        | Accès global aux microservices    | **8888** | -               |

**Accès rapides :**

- Eureka : http://localhost:8761  
- Clients via Gateway : http://localhost:8888/SERVICE-CLIENT/api/client  
- Cars via Gateway : http://localhost:8888/SERVICE-CAR/api/car  

---

## 3. Eureka Server

### Configuration (`application.yml`)
- Port : 8761  
- Désactivation de l’enregistrement client  
- Activation du serveur Eureka  

### Classe principale
```java
@EnableEurekaServer
@SpringBootApplication
public class EurekaServerApplication {}
```
## 4. Microservice SERVICE-CLIENT

### Fonctionnalités
- CRUD clients (id, nom, âge…)
- Inscription automatique dans Eureka

### Technologies
- Spring Web  
- Spring Data JPA + MySQL  
- Eureka Client  
- Lombok  

### Configuration
- Port : **8081**
- Nom : `SERVICE-CLIENT`
- Base : `clientservicedb`

### Structure
- **Entity** : `Client`
- **Repository** : `ClientRepository`
- **Service**
- **Controller** : `/api/client`

---

## 5. Microservice SERVICE-CAR

### Fonctionnalités
- CRUD voiture  
- Récupération du propriétaire via SERVICE-CLIENT (REST call)

### Technologies
- Identiques au service client

### Configuration
- Port : **8082**
- Nom : `SERVICE-CAR`
- Base : `carservicedb`

### Structure
- **Entity** : `Car`
- **DTO** : `Client`
- **Response** : `CarResponse`
- **RestTemplate** : communication inter-services
- **Controller** : `/api/car`

---

## 6. API Gateway

### Rôle
- Point d’entrée unique
- Routage dynamique via Eureka

### Configuration (`application.yml`)
- Port : **8888**
- Routage basé sur noms de services (`SERVICE-CLIENT`, `SERVICE-CAR`)

---

## 7. Démarrage (ordre conseillé)
1. MySQL  
2. Eureka Server  
3. SERVICE-CLIENT  
4. SERVICE-CAR  
5. Gateway  

---

## 8. Tests

### Sans Gateway :
- Clients : `http://localhost:8081/api/client`  
- Cars : `http://localhost:8082/api/car`  

### Avec Gateway :
- Clients : `http://localhost:8888/SERVICE-CLIENT/api/client`  
- Cars : `http://localhost:8888/SERVICE-CAR/api/car`  
---

## 9. Captures d’écran importantes

### 1. Eureka Server – Dashboard
Aperçu des microservices enregistrés dans Eureka.

<img width="1658" height="930" alt="Screenshot 2025-12-04 143657" src="https://github.com/user-attachments/assets/5e2e8186-32fc-490e-8fef-2f1dd92de489" />

---

### 2. SERVICE-CLIENT – Liste des clients
Exemple de la réponse du microservice client.

<img width="607" height="904" alt="image" src="https://github.com/user-attachments/assets/7e5a5d2d-90cf-48f1-b614-3eb154d6958c" />

---

### 3. SERVICE-CAR – Voiture + Propriétaire (Client)
Réponse enrichie après appel REST vers SERVICE-CLIENT.
<img width="516" height="691" alt="image" src="https://github.com/user-attachments/assets/63bc5615-5283-4b89-882b-29052992abdf" />


---

### 5. MySQL – Tables générées
Visualisation des tables `client` et `car`.

<img width="1271" height="418" alt="image" src="https://github.com/user-attachments/assets/1e3d4fca-b1f1-49e7-8ebf-df6bfa70e88c" />
<img width="1279" height="322" alt="image" src="https://github.com/user-attachments/assets/084e65c0-481e-430d-a1bf-1880cc938adb" />

