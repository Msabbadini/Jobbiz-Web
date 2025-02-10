# 🎯 Back End ( Version : Juin 2024)

## 📖 Description
Ce projet de backend offre une API sécurisée permettant de gérer l'authentification des utilisateurs, les rôles, et la gestion des comptes. Il comprend la validation des informations d'inscription, la gestion des erreurs, et l'envoi d'emails de validation. L'API assure aussi la redirection des utilisateurs en fonction de leurs rôles (entreprise ou intérimaire) vers les sites appropriés.

Objectif : Fournir une API sécurisée et scalable pour gérer les utilisateurs et leurs interactions avec la plateforme Jobbiz.
Besoins adressés : Gestion d'authentification, validation des données, gestion des rôles, sécurité des comptes, et communication par email.
Contexte : Développement d'un backend pour un projet interne de Jobbiz dans un environnement de production.

## 🚀 Fonctionnalités principales
✅ Inscription utilisateur : Validation des informations (email, mot de passe), création d’un compte sécurisé.      
✅ Connexion utilisateur : Authentification avec JWT, gestion des rôles (redirection vers le site approprié).       
✅ Validation par email : Envoi d'un email de confirmation avec un token pour activer le compte utilisateur.        
✅ Gestion des erreurs : Gestion centralisée des erreurs API, avec des messages clairs et explicites pour l’utilisateur.        
✅ Récupération mot de passe : Fonctionnalité de réinitialisation de mot de passe avec validation par email.        
✅ Demande de démonstration : Envoi d’un formulaire de demande pour les entreprises, traitement côté serveur.       

## 🛠️ Technologies utilisées

Node.js / Express.js : Framework serveur pour créer l'API backend.      
JWT (JSON Web Tokens) : Pour l'authentification et la gestion des sessions utilisateurs.        
Bcrypt : Pour le hachage sécurisé des mots de passe.        
MongoDB / PostgreSQL : Pour le stockage des utilisateurs et des données (choisir selon ta base de données).     
Nodemailer : Pour l'envoi d’emails (activation de compte, réinitialisation de mot de passe).        
Swagger / Postman : Pour la documentation et les tests des API.     
Jest / Mocha : Pour les tests unitaires et l'intégration des API.       

## 🚀 Packages principaux utilisés :
Voici une liste des packages clés utilisés dans ce backend :

**express** : Framework pour créer l'API et gérer les requêtes HTTP.        
**jsonwebtoken** (JWT) : Pour générer et vérifier les tokens d'authentification, assurant la sécurité des sessions utilisateur.     
**bcrypt** : Pour le hachage sécurisé des mots de passe avant leur stockage dans la base de données.        
**mongoose** : ORM pour interagir avec MongoDB, facilitant la gestion des données et des requêtes.      
**mysql2** : Client MySQL pour interagir avec des bases de données SQL, offrant une interface rapide et légère.     
**axios** : Client HTTP pour effectuer des requêtes asynchrones vers des API externes.      
**cookie**-parser : Middleware pour analyser les cookies attachés aux requêtes HTTP.        
**dotenv** : Pour gérer les variables d'environnement et les configurations sensibles en toute sécurité.        
**cors** : Middleware pour gérer les permissions de partage des ressources entre différents domaines.       
**express-fileupload** : Middleware pour le téléchargement de fichiers via des requêtes HTTP multipart/form-data.       
**sharp** : Bibliothèque pour le traitement des images, comme le redimensionnement ou la conversion de formats.     
**sentry/node** : Outil de monitoring et de gestion des erreurs, permettant de suivre les exceptions et les performances.       
**onesignal-node** : SDK pour intégrer les notifications push via OneSignal dans l'application.     
**stripe** : Service de paiement en ligne pour gérer les transactions sécurisées sur le site.       
**firebase-admin** : SDK pour administrer Firebase, incluant l'authentification, la base de données en temps réel et les notifications push.        
**socket.io** : Bibliothèque pour ajouter la communication en temps réel via WebSockets.        
**multer** : Middleware pour gérer les téléchargements de fichiers dans les requêtes HTTP.      
**node-cron** : Utilisé pour planifier et exécuter des tâches répétitives à des moments spécifiques (cron jobs).        
**validator** : Bibliothèque de validation pour vérifier les données d'entrée comme les emails, les URL, etc.       
**sib-api-v3-sdk** : SDK pour intégrer Sendinblue, un service d'envoi d'emails transactionnels et marketing.        
**xml2js** : Bibliothèque pour convertir des données XML en objets JavaScript, facilitant le traitement de fichiers XML.        
**base64topdf** : Utilitaire pour convertir des chaînes base64 en fichiers PDF.     
**cheerio** : Bibliothèque pour manipuler le HTML, semblable à jQuery, utilisée pour le scraping ou la manipulation de contenu HTML.        
**compression** : Middleware pour compresser les réponses HTTP et améliorer la performance de l'application.
**convertapi** : Service de conversion de fichiers via une API, permettant de convertir des documents en différents formats.        
**form-data** : Pour la gestion et l'envoi de formulaires multipart/form-data, souvent utilisé avec les fichiers.       
**json-bigint** : Permet de gérer les nombres de grande taille dans les fichiers JSON sans perte de précision.      

## 📂 Structure du projet
La structure du projet suit une organisation modulaire et claire afin de faciliter le développement, la maintenance et l'extensibilité. Voici un aperçu des principaux dossiers et fichiers du backend :

 <img src="assets/structure.png" width="auto" height="auto" />


## 📝 Description des dossiers principaux :
### config
Ce dossier contient les fichiers de configuration de l'application. Il regroupe les paramètres nécessaires au bon fonctionnement du projet, comme les connexions à la base de données, les variables d'environnement et d'autres paramètres globaux.

### controllers
Les contrôleurs gèrent la logique métier de l'application. Ils prennent en charge les requêtes, traitent les données et renvoient des réponses appropriées. Ce dossier contient les fichiers où chaque contrôleur est responsable d'une fonctionnalité spécifique.

### Media
Ce dossier est dédié aux fichiers multimédia utilisés dans le projet. Cela inclut les images, documents et autres fichiers qui sont manipulés ou stockés par l'application.

### Middleware
Le dossier middleware contient les composants qui s'intercalent entre la requête et la réponse. Ces éléments permettent de gérer les processus transverses comme l'authentification, la validation des données ou encore la gestion des fichiers avant de parvenir à un contrôleur.
Consultez l'exemple [ici](docs/api.md).

### Models
Ce dossier regroupe les définitions des modèles de données. Chaque modèle représente une structure de données qui est généralement liée à une table de la base de données et contient la logique nécessaire pour interagir avec ces données.

### Routes
Les routes définissent les points d'entrée de l'application, associant des URLs à des contrôleurs et actions spécifiques. Ce dossier contient les fichiers où les routes sont définies et gèrent les demandes HTTP entrantes.

### Services
Ce dossier regroupe la logique métier réutilisable à travers l'application. Les services contiennent des fonctions qui exécutent des opérations plus complexes et qui sont souvent appelées depuis les contrôleurs pour gérer des tâches comme l'envoi d'e-mails, la manipulation de données ou la gestion des utilisateurs.

### Utils
Le dossier utils contient des fonctions utilitaires et des outils génériques utilisés dans toute l'application. Ces fonctions facilitent la gestion de tâches courantes comme la validation de données, la manipulation de dates ou encore des transformations de données.


## ⚡ Architecture de l'API
Consultez la documentation complète de l'API [ici](docs/api.md).


## 📈 Sécurité
Hachage des mots de passe avec Bcrypt.      
Token expiration et révocation avec JWT.        
Validation des données (email, mot de passe, etc.) avant l’enregistrement en base de données.       
Protection contre les attaques de type injection SQL ou XSS.        

## 🔧 Tests et Validation
Tests d'intégration pour vérifier le bon fonctionnement des endpoints avec des outils comme Postman.        
Tests de performance pour vérifier la scalabilité de l’API.     

[Retour à la documentation principale](../../README.md)