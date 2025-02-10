# ⚡ Architecture de l'API

## **Endpoints principaux**

L'API de Jobbiz est construite autour de plusieurs routes RESTful, chacune servant un ensemble spécifique de fonctionnalités. Voici un aperçu des différentes routes disponibles :

**/{YOUR_APP}/files** : Serve les fichiers statiques de l'application.     
**/{YOUR_APP}/upload**: Gère les opérations liées au téléchargement de fichiers.      
**/{YOUR_APP}/refresh**: Permet de rafraîchir les tokens d'authentification.      
**/{YOUR_APP}/address**: Gère les opérations liées aux adresses.
**/{YOUR_APP}/city**: Route pour la gestion des villes.       
**/{YOUR_APP}/company** : Permet de gérer les informations liées aux entreprises.      
**/{YOUR_APP}/convention** : Gestion des conventions et contrats.
**/{YOUR_APP}/employees** : Permet la gestion des employés dans l'entreprise.      
**/{YOUR_APP}/holding**: Gestion des holdings.        
**/{YOUR_APP}/language**: Routes dédiées à la gestion des langues disponibles dans l'application.     
**/{YOUR_APP}/licence**: Permet de gérer les licences utilisateur.        
**/{YOUR_APP}/mission**: Routes pour la gestion des missions des employés.        
**/{YOUR_APP}/tools**: Contient les outils nécessaires pour l'application.        
**/{YOUR_APP}/user**: Gère les opérations liées aux utilisateurs.     
**/{YOUR_APP}/tempWorker** : Pour la gestion des travailleurs temporaires.     
**/{YOUR_APP}/resend**: Permet de renvoyer des emails ou des notifications.       
**/{YOUR_APP}/token** : Routes dédiées à la gestion des tokens de sécurité.        
**/{YOUR_APP}/department** : Gère les départements au sein de l'entreprise.        
**/{YOUR_APP}/listes**: Permet de gérer les listes, telles que les listes de départements, employés, etc.     
**/{YOUR_APP}/timeSheets** : Gère les feuilles de temps des employés.      
**/{YOUR_APP}/admin** : Routes réservées aux administrateurs pour la gestion complète de l'application.
**/{YOUR_APP}/support**: Permet d'accéder aux services de support.        
**/{YOUR_APP}/schedule**: Gestion des plannings et horaires des employés.     
**/{YOUR_APP}/mobile**: Routes liées à l'application mobile.
**/{YOUR_APP}/hrFlowJobs** : Permet la gestion du flux des ressources humaines liées aux emplois.      
**/{YOUR_APP}/mobilePro** : Routes pour les fonctionnalités professionnelles de l'application mobile.      
**/{YOUR_APP}/vivier**: Gestion des viviers de candidats.     
**/{YOUR_APP}/qualification** : Routes pour gérer les qualifications des employés.      


## **Middleware**

Les middlewares sont des fonctions intermédiaires qui s'exécutent pendant le traitement des requêtes, avant de parvenir à l'action finale du contrôleur. Ils sont souvent utilisés pour des tâches telles que l'authentification, la validation, ou encore le traitement des fichiers. Voici un exemple d'utilisation d'un middleware pour gérer le téléchargement et la modification d'un fichier :

```javascript
router.post('/singleFile/CV', 
  uploadCv.single('file'), // Middleware pour gérer le téléchargement du fichier
  resizeImage,             // Middleware pour redimensionner l'image après le téléchargement
  filesController.singleFileUpload // Fonction contrôleur qui gère la sauvegarde du fichier
);
```

### Upload fichier
```javascript
// Document
/**
 * Configuration de stockage Multer pour la gestion des téléchargements de fichiers CV.
 * 
 * @constant
 * @type {import('multer').StorageEngine}
 * 
 * @property {Function} destination - Définit le répertoire de destination pour les fichiers téléchargés.
 * @param {Object} req - L'objet de la requête.
 * @param {Object} file - L'objet fichier contenant les détails du fichier.
 * @param {Function} cb - La fonction de rappel pour définir le répertoire de destination.
 * 
 * @property {Function} filename - Définit le nom de fichier pour les fichiers téléchargés.
 * @param {Object} req - L'objet de la requête.
 * @param {Object} file - L'objet fichier contenant les détails du fichier.
 * @param {Function} cb - La fonction de rappel pour définir le nom de fichier.
 */
const storageCv = multer.diskStorage({
	destination: (req, file, cb) => {
		cb(null, 'media');
	},
	filename: (req, file, cb) => {
		// cb(null, new Date().toISOString().replace(/:/g, '-') + '-' + slugify(file.originalname));
		cb(null, `cv.${typeFile(file.mimetype)}`)
	}
});

/**
 * Middleware pour télécharger un CV.
 * Utilise multer pour gérer le stockage et le filtrage des fichiers.
 *
 * @type {multer.Instance}
 */
const uploadCv = multer({ storage: storageCv, fileFilter: filefilter });
```

### Redimension de l'image
```javascript
/**
 * Middleware pour redimensionner une image téléchargée.
 * 
 * @param {Object} req - L'objet de requête Express.
 * @param {Object} req.file - Le fichier téléchargé.
 * @param {string} req.file.path - Le chemin du fichier téléchargé.
 * @param {string} req.file.mimetype - Le type MIME du fichier téléchargé.
 * @param {string} req.file.originalname - Le nom original du fichier téléchargé.
 * @param {Object} res - L'objet de réponse Express.
 * @param {Function} next - La fonction middleware suivante dans la chaîne.
 * 
 * @returns {void}
 */
const resizeImage = (req, res, next) => {
	if (req.file && req.file.path) {
		// Vérifiez si le type de fichier est une image (jpeg, png, etc.)
		if (!req.file.mimetype.startsWith('image')) {
			return next();
		}
		const cheminImage = req.file.path; // Obtenez le chemin de l'image depuis req.file.path
		const cheminTemporaire = path.join('media/', 'temp_' + req.file.originalname); // Créez un fichier temporaire

		sharp(cheminImage)
			.metadata() // Obtenez les métadonnées de l'image pour obtenir la largeur et la hauteur
			.then(metadata => {
				const nouvelleLargeur = Math.round(metadata.width * 0.8);
				const nouvelleHauteur = Math.round(metadata.height * (nouvelleLargeur / metadata.width)); // Calculez la hauteur proportionnellement

				// Redimensionnez l'image
				sharp(cheminImage)
					.resize()
					.toFile(cheminTemporaire, (err, info) => {
						if (err) {
							console.error('Erreur lors du redimensionnement de l\'image :', err);
							next();
						} else {
							// Vérifiez si la largeur est plus grande que la hauteur après le redimensionnement
							if (nouvelleLargeur > nouvelleHauteur) {

								sharp(cheminTemporaire)
									.rotate(90)  // Ajustez l'orientation en fonction des métadonnées EXIF
									.toFile(cheminImage, (rotateErr, rotateInfo) => {
										if (rotateErr) {
											console.error('Erreur lors de la rotation de l\'image :', rotateErr);
											next();
										} else {
											next();
										}
									});
							} else {
								// Aucune rotation nécessaire
								fs.rename(cheminTemporaire, cheminImage, (renameErr) => {
									if (renameErr) {
										console.error('Erreur lors du renommage du fichier temporaire :', renameErr);
										next();
									}
									console.log('Image redimensionnée avec succès ! Nouvelles informations sur le fichier :', info);
									next();
								});
							}
						}
					});
			})
			.catch(err => {
				console.error('Erreur lors de la récupération des dimensions de l\'image :', err);
				next();
			});
	} else {
		next();
	}
};
```
Authentication : Vérification du token JWT dans les headers pour sécuriser l'accès aux routes.      
Authorization : Vérification des rôles pour rediriger vers le bon site.
Gestion des erreurs     

Erreurs de validation (ex : format d'email incorrect, mot de passe trop faible).        
Erreurs d’authentification (ex : token invalide).       
Erreurs système (ex : problème avec la base de données).        

[Retour à la documentation backend](../README.md)       
[Retour à la documentation principale](../../README.md)