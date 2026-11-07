# Politique de confidentialité — OTOTEKA

**Version du document :** 6 novembre 2026  
**Application :** OTOTEKA (nom affiché sur le téléphone : Ototeka)  
**Paquet Android :** `com.autoteka.app`

Ce texte décrit le comportement présent dans le code de l’application. Il ne décrit pas de publicité, d’analyse d’audience, ni de paiement en ligne, car ces fonctions ne sont pas dans l’application.

## Informations à confirmer avant publication

Les éléments suivants ne figurent pas dans le code. Ils ne sont pas inventés ici.

| Sujet | Situation |
|---|---|
| Raison sociale, forme juridique, numéro d’immatriculation | **À confirmer.** Le code ne contient que le nom du produit « Ototeka ». |
| Adresse du siège | **À confirmer.** L’application affiche « Avenue Kasavubu, Lubumbashi, RDC ». Confirmez qu’il s’agit bien de votre adresse avant de la présenter comme siège. |
| E-mail de confidentialité | **À confirmer.** La seule adresse dans l’application est `hello@autoteka.cd`. Confirmez qu’elle est lue. |
| Téléphone et WhatsApp | **À confirmer.** L’application affiche `+243 990 000 123` et `https://wa.me/243990000123`. |
| Région d’hébergement Supabase et chiffrement des disques | **À confirmer** dans le tableau de bord Supabase. Le code de l’application ne le définit pas. |
| Envoi réel des e-mails via Resend | **À confirmer** sur le projet Supabase utilisé par l’application publiée. |
| Présence de l’outil optionnel de détourage d’image (`REMBG_API_URL`) dans le fichier publié | **À confirmer** dans les paramètres de compilation de l’App Bundle. |
| Application de la fonction `delete_my_account` sur le projet Supabase de production | **À confirmer.** La migration existe dans le dépôt. |

Jusqu’à confirmation, cette page parle du **produit OTOTEKA** et des **coordonnées affichées dans l’application**, pas d’une société dont l’identité aurait été vérifiée.

## 1. Qui est concerné

Cette politique concerne l’application Android OTOTEKA et le service Supabase auquel elle est reliée : authentification, base de données Postgres, stockage de fichiers, et une fonction d’envoi d’e-mails.

Le même dépôt contient une console d’administration web. Elle sert aux opérateurs (validation des boutiques et des documents, activation manuelle d’un accès vendeur). Elle n’est pas l’application proposée au public sur Google Play.

## 2. Ce qu’est OTOTEKA

OTOTEKA est une marketplace de pièces automobiles, destinée à Lubumbashi. Le catalogue des boutiques approuvées peut être consulté sans compte.

Un compte est une adresse e-mail et un mot de passe. Il n’existe pas de compte « acheteur » distinct d’un compte « vendeur ». Une personne connectée peut créer une boutique, publier des pièces, enregistrer un véhicule ou déposer une recherche spéciale.

L’application n’encaisse pas le prix de la pièce et n’organise pas la livraison. Le contact avec le vendeur se fait par WhatsApp ou par le téléphone de l’utilisateur.

## 3. Données des visiteurs et des acheteurs

### Sans compte

La consultation des annonces en ligne n’exige pas de compte. Aucune position GPS n’est demandée.

### Compte

- Adresse e-mail, obligatoire pour l’inscription et la connexion.
- Mot de passe, envoyé à Supabase Auth. Il n’est pas recopié dans les tables de la marketplace.
- Le formulaire d’inscription ne demande pas le nom de la personne.
- La connexion par numéro de téléphone n’existe pas.

La confirmation d’e-mail et la réinitialisation du mot de passe sont prévues. Les messages passent par Supabase Auth. Le dépôt documente Resend comme service d’envoi. Le fait que Resend soit réellement branché sur le projet publié reste **à confirmer**.

### Recherche spéciale

Une personne connectée qui publie une recherche fournit :

- un titre et une description ;
- au moins une photo ;
- la marque, le modèle, l’année, la version et la catégorie de pièce ;
- un lieu en texte libre (le formulaire propose « Lubumbashi » par défaut) ;
- un numéro de téléphone ;
- le choix d’être contacté par WhatsApp, par appel, ou les deux selon l’écran.

Le numéro est enregistré à part. Les lectures directes de cette table sont retirées aux clients. Un vendeur ne reçoit le numéro que par une fonction du serveur, et seulement si sa boutique correspond à la demande et qu’un administrateur a activé une période d’accès.

Les photos de la recherche vont dans un espace de stockage privé (`special-search-photos`).

### Véhicule et VIN

- Une photo de VIN peut être lue sur le téléphone par un examen simple du fichier. Cette photo n’est pas envoyée pour la lecture du VIN. Ce n’est pas un moteur d’OCR complet.
- Si le décodage continue, l’application envoie le VIN au service public NHTSA : `https://vpic.nhtsa.dot.gov`.
- L’application peut conserver le VIN, la description du véhicule et la réponse du décodeur dans un cache (`vin_lookups`). Ce cache n’est pas lié à l’identifiant du compte. La politique de base actuelle permet à un visiteur non connecté de lire ces lignes.
- L’utilisateur peut enregistrer un véhicule dans « Mon garage », avec un VIN facultatif et un surnom facultatif. Ces lignes ne sont lisibles que par le compte concerné.

### Ce qui n’est pas collecté

Le code de l’application ne collecte pas :

- la position GPS du téléphone ;
- le carnet d’adresses, les SMS ou le microphone ;
- un identifiant publicitaire ;
- des statistiques d’usage ou des rapports de plantage via un SDK ;
- de la publicité ;
- un paiement par carte ou par mobile money dans l’application ;
- un nom d’affichage d’acheteur, un historique de commandes ou une liste de favoris.

## 4. Données des vendeurs

La création d’une boutique enregistre :

- le nom de la boutique, le nom du responsable et une description ;
- l’e-mail du compte, recopié sur la boutique ;
- le numéro de téléphone et un lien WhatsApp produit à partir de ce numéro (`https://wa.me/…`) ;
- la ville, le marché et la rue, en texte (le formulaire propose Lubumbashi et Kenya par défaut) ;
- le type de vendeur : particulier ou entreprise ;
- un logo facultatif ;
- le statut de la boutique (par exemple en attente, approuvée, suspendue, rejetée).

Une boutique approuvée peut être lue sans compte. Cette lecture comprend le téléphone, l’e-mail, le nom du responsable, l’adresse textuelle et le lien WhatsApp.

Une annonce enregistre le titre, la catégorie, le prix, la devise, la description, l’état, la référence, le véhicule, un lieu en texte, le nom de la boutique, le téléphone du vendeur, le lien WhatsApp et les photographies.

Les annonces en ligne d’une boutique approuvée sont lisibles sans compte. Les photos de pièces sont dans un espace marqué public dans le schéma (`product-images`). Les logos sont dans `shop-assets`, également marqué public. Une modification ultérieure limite le listage de ces fichiers par l’API au propriétaire ou à un administrateur. Une adresse publique du fichier reste accessible à qui la connaît.

### Pièce d’identité et RCCM

Le vendeur peut envoyer :

- une carte d’identité, un passeport, ou une « Identité Nationale » déjà acceptée comme ancien type ;
- un RCCM pour une société ;
- une patente, un NIF, ou un document classé « Autre » ;
- un PDF ou une image.

Ces fichiers vont dans l’espace privé `shop-documents`. Ils servent à la vérification de la boutique. L’application ne fait pas de reconnaissance faciale automatique.

Une nouvelle annonce d’une boutique déjà approuvée est publiée avec le statut « en ligne ». Une annonce d’une boutique non approuvée reste en examen.

## 5. Pourquoi ces données sont utilisées

- Créer le compte, confirmer l’e-mail et permettre la réinitialisation du mot de passe.
- Afficher les boutiques et les pièces.
- Ouvrir WhatsApp ou le composeur téléphonique lorsque l’utilisateur appuie sur le bouton.
- Examiner les documents et le statut des boutiques.
- Rapprocher une recherche spéciale des boutiques concernées et, si l’accès est actif, montrer le numéro de l’acheteur à ce vendeur.
- Décoder un VIN et conserver les véhicules enregistrés par l’utilisateur.
- Envoyer les e-mails de confirmation, de mot de passe et de suivi de modération lorsque ce service est configuré.

L’application ne contient pas de SDK publicitaire. Le code ne vend pas les données personnelles comme un fichier commercial.

## 6. Où les données sont traitées

| Destinataire | Données | Moment |
|---|---|---|
| Supabase (Auth, Postgres, Storage) | Données de compte et de marketplace enregistrées par l’application | Lorsque l’application publiée est configurée avec un projet Supabase |
| Resend | Adresse e-mail et contenu du message | Si le SMTP Auth ou la fonction `send-transactional-email` est configuré. **À confirmer** sur le projet publié |
| NHTSA (États-Unis) | Le VIN uniquement | Lorsque l’utilisateur lance un décodage |
| WhatsApp | Le numéro de destination et, pour une annonce, un message prérempli avec le nom de la pièce et le prix | Uniquement après un appui sur le bouton WhatsApp. Il n’y a pas d’API WhatsApp dans l’application |
| Composeur du téléphone | Le numéro affiché | Uniquement après un appui sur appeler |
| Outil optionnel de détourage | Les octets d’une photo de pièce | Seulement si `REMBG_API_URL` est compilé dans la version publiée. Les logos de boutique ne sont pas envoyés à cet outil. **À confirmer** |
| Google Fonts | Demande de fichiers de la police Inter | Sur les écrans qui utilisent cette police. Le code n’ajoute pas l’e-mail du compte à cette demande |
| Autres utilisateurs | Coordonnées des boutiques approuvées et annonces en ligne ; texte et photos d’une recherche spéciale pour les vendeurs correspondants ; numéro de l’acheteur seulement pour un vendeur dont l’accès est actif | Comme décrit ci-dessus |
| Administrateurs | Fiches boutique, documents, annonces et contacts de recherche spéciale | Par les outils d’administration du projet |

Des notes de configuration parlent de Twilio. L’application Android n’appelle pas Twilio.

## 7. Photos et documents

| Fichier | Espace de stockage | Accès prévu dans le schéma |
|---|---|---|
| Photos de pièces | `product-images` | Public |
| Logo de boutique | `shop-assets` | Public |
| Pièce d’identité, RCCM et documents associés | `shop-documents` | Privé |
| Photos de recherche spéciale | `special-search-photos` | Privé |

## 8. Sécurité des communications

Ce que le code vérifie :

- le trafic HTTP non chiffré est interdit sur Android, sauf vers `localhost`, `10.0.2.2` et `127.0.0.1` ;
- les adresses NHTSA et WhatsApp utilisées par l’application commencent par `https://` ;
- la sauvegarde Android des données de l’application est désactivée.

Ce que le code ne vérifie pas :

- l’adresse Supabase est fournie à la compilation (`SUPABASE_URL`). Le source n’oblige pas cette adresse à commencer par `https://` et n’épingle pas de certificat ;
- l’application ne chiffre pas elle-même les disques du serveur.

Le chiffrement au repos chez Supabase est **à confirmer** dans les réglages du projet. Cette politique ne l’affirme pas.

Autres mesures présentes dans le code : contrôle d’accès par ligne sur les tables de la marketplace, documents d’identité dans un espace privé, numéros de recherche spéciale non lisibles par une simple requête de table, mot de passe de 8 à 72 caractères avec minuscule, majuscule, chiffre et caractère spécial, mode démonstration interdit dans les versions release.

Ces mesures ne constituent pas une garantie contre toute intrusion.

## 9. Suppression du compte

Depuis l’application : **Mon profil → Supprimer mon compte**. L’application appelle la fonction `delete_my_account`.

Cette fonction, lorsqu’elle est déployée :

- supprime l’utilisateur d’authentification ;
- supprime la boutique, ce qui entraîne les annonces et les lignes de documents de la boutique ;
- tente de supprimer les fichiers de cette personne dans `shop-assets`, `product-images` et `shop-documents` ;
- supprime les véhicules enregistrés (`user_vehicles`) ;
- supprime les recherches spéciales de cette personne, donc aussi le numéro de téléphone lié à ces recherches.

Elle ne supprime pas les fichiers de `special-search-photos`. Elle ne supprime pas le cache VIN (`vin_lookups`), car ces lignes n’appartiennent pas à un compte. Une erreur pendant le nettoyage des fichiers n’empêche pas la suppression du compte d’authentification.

La page [Demander la suppression du compte](suppression.html) explique la même procédure pour une personne qui n’est pas dans l’application.

Si la personne ne peut plus se connecter, elle peut demander un nouveau mot de passe depuis l’écran de connexion, puis supprimer le compte. Elle peut aussi écrire à l’adresse affichée dans l’application.

## 10. Conservation

Le dépôt ne contient pas de tâche qui efface les comptes, les photos, le cache VIN ou les documents d’identité après un nombre de jours fixe.

Une annonce peut être marquée retirée ou archivée. C’est un changement de statut, pas un effacement automatique du fichier image.

Un administrateur peut marquer une période d’accès vendeur comme expirée. Cela ne supprime pas la fiche du vendeur.

La durée de vie d’un lien de confirmation d’e-mail dépend de Supabase et du service d’e-mail. Ce n’est pas une suppression de compte.

Le cache VIN est conservé parce qu’il n’est pas rattaché à une personne : le supprimer lors d’une suppression de compte effacerait un résultat partagé avec d’autres utilisateurs.

## 11. Mineurs

OTOTEKA est une marketplace commerciale. Elle peut recueillir une pièce d’identité et un numéro de téléphone. Elle n’est pas conçue pour les enfants. Le code ne contient pas de compte enfant.

## 12. Abonnement vendeur

Un écran « Abonnement » explique qu’un accès mensuel, activé par un administrateur après un contact WhatsApp ou e-mail, permet de voir le numéro des acheteurs d’une recherche spéciale. L’application ne demande pas de carte bancaire et n’intègre pas de paiement. Le paiement en ligne est décrit à l’écran comme une version ultérieure.

## 13. Modifications

Cette page sera mise à jour si le comportement réel change, par exemple si un paiement dans l’application ou des notifications push sont ajoutés. La version datée ci-dessus correspond au code examiné le 6 novembre 2026.

## 14. Contact

Coordonnées **affichées dans l’application** (à confirmer comme contact réel avant usage public) :

- E-mail : [hello@autoteka.cd](mailto:hello@autoteka.cd)
- Téléphone : +243 990 000 123
- WhatsApp : [https://wa.me/243990000123](https://wa.me/243990000123)
- Lieu affiché : Avenue Kasavubu, Lubumbashi, République démocratique du Congo

Pour une demande liée aux données personnelles, écrivez à l’adresse e-mail ci-dessus avec l’objet « Confidentialité OTOTEKA » et l’adresse e-mail du compte concerné.
