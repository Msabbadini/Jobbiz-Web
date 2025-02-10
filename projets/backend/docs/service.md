# 📦 Services

## Exemple de service : Envoi de notifications via OneSignal

**Fonction** : *sendNotificationOneTemplate*    
Cette fonction prend en entrée une liste de tokens, le type de template de notification à utiliser, et des paramètres supplémentaires pour personnaliser l'URL de la notification si nécessaire.

**Code** :
```javascript
const https = require('https');

exports.sendNotificationOneTemplate = function (
    listToken,
    typeTemplate,
    param,
) {
    let templateNotification = "";
    let urlNotification = "";

    // Choix du template basé sur le type
    switch (typeTemplate) {
        case "newMission":
            templateNotification = "template_id_mission";
            break;
        case "fileRefused":
            templateNotification = "template_id_refus";
            break;
        case "contratDispo":
            templateNotification = "template_id_contrat";
            urlNotification = `url_jobbiz_notification`;
            break;
        // Autres templates...
        default:
            console.error("Type de template non reconnu:", typeTemplate);
            return;
    }

    // Fonction pour envoyer la notification
    const sendNotification = function (data) {
        const headers = {
            "Content-Type": "application/json; charset=utf-8",
            Authorization: `Basic 'YOUR_APP_ID'}`,
        };

        const options = {
            host: "onesignal.com",
            port: 443,
            path: "/api/v1/notifications",
            method: "POST",
            headers: headers,
        };

        const req = https.request(options, function (res) {
            let responseData = '';

            res.on("data", function (chunk) {
                responseData += chunk;
            });

            res.on('end', function () {
                console.log("Réponse reçue:");
                try {
                    const parsedData = JSON.parse(responseData);
                    console.log(parsedData);
                } catch (e) {
                    console.error("Échec du parsing JSON:", e);
                }
            });
        });

        req.on("error", function (e) {
            console.log("Erreur:");
            console.log(e);
        });

        req.write(JSON.stringify(data));
        req.end();
    };

    // Message à envoyer
    const message = {
        app_id: 'YOUR_APP_ID',
        template_id: templateNotification,
        channel_for_external_user_ids: "push",
        include_external_user_ids: listToken,
    };

    // Si un paramètre d'URL est fourni
    if (param && urlNotification) {
        message.url = urlNotification;
    }

    // Envoi de la notification
    sendNotification(message);
};

```
### Explication :   
**Paramètres d'entrée** :

**listToken** : Liste des tokens des utilisateurs à qui envoyer la notification.
typeTemplate : Type de notification (ex. "newMission", "fileRefused", etc.).    
**param** : Paramètre supplémentaire (si nécessaire) pour personnaliser l'URL de la notification.

**Fonctionnement** :
Selon le typeTemplate, un template de notification est choisi et l'URL est éventuellement ajoutée si un paramètre est fourni (par exemple pour afficher un document ou une mission).    
Une requête HTTPS est envoyée à l'API OneSignal pour envoyer la notification à la liste des tokens.     

**Sécurité** :  
La clé API OneSignal est stockée dans les variables d'environnement (process.env.ONESIGNAL_API_KEY), garantissant que la clé n'est pas exposée dans le code.        

**Utilisation** :   
Tu peux appeler cette fonction depuis n'importe quel service ou contrôleur en lui fournissant une liste de tokens et le type de notification. Par exemple :

```javascript
sendNotificationOneTemplate([userToken1, userToken2], "newMission", null);
```

[Retour à la documentation backend](../README.md)       
[Retour à la documentation principale](../../README.md)