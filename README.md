# DRAFT — Site de présentation

Site vitrine **statique**, en français (/) et en anglais (/en/), indépendant du dépôt de l'application Windows [DRAFT](https://github.com/lucasmettetal/Draft).

## Démarrage local

Ouvrir `index.html` dans un navigateur, ou lancer :

```sh
python -m http.server 8080
```

Puis visiter `http://localhost:8080`.

## Déploiement Vercel

Importer ce dépôt dans Vercel (« Add New Project »), sélectionner le dépôt `draft-website`, choisir le preset **Other** et laisser la commande de build vide. Dossier racine `.`. Le fichier `vercel.json` ajoute quelques en-têtes de sécurité.

Le déploiement est indépendant de l'application Windows.

## Domaine et vérification Riot

Choisir un domaine que vous contrôlez et l'associer à Vercel. Si Riot vous remet un fichier de vérification `riot.txt`, créer ce fichier **à la racine** avec le contenu **exactement fourni**, déployer, puis vérifier que `https://votre-domaine/riot.txt` renvoie le bon contenu. Ne pas inventer le jeton.

## Dossier Riot

`docs/riot/` contient le dossier de candidature à l'accès Production de l'API Riot Games (en anglais) : description du produit, inventaire des appels, questions de conformité, liste des éléments. Ce dossier n'est pas publié sur le site (`.vercelignore`).

## Vidéo

`assets/draft-tutoriel.mp4` (environ 16 Mo, lecture uniquement au clic) et ses sous-titres `assets/draft-tutoriel-fr.vtt` / `-en.vtt`.

## À compléter avant publication générale

- [ ] Identité de l'éditeur et mentions légales selon le statut (non publiées : à décider, ne rien inventer).
- [ ] Adresse de contact dédiée aux demandes concernant les données.
- [ ] Validation des textes de confidentialité et conditions contre la version réellement diffusée.
- [x] Vraies captures de DRAFT 2.16, avec une draft simulée et des données fictives (ssets/).
- [x] Adresse de contact : lucas.mettetal2@gmail.com.
- [ ] Serveur de statistiques avec la clé de production, tel que décrit dans la section API du site, ou retirer ce paragraphe.
- [ ] Vérifier avec Riot les usages de la LCU, les analyses de sélection et les conditions de distribution.
- [ ] Confirmer la disponibilité du nom et du domaine DRAFT avant toute diffusion de grande ampleur.

## Confidentialité technique du site

Pas de cookies de suivi ni d'analytics applicatifs. Les polices Google Fonts entraînent potentiellement une requête à un tiers. Vercel peut traiter les journaux d'accès selon ses propres conditions.

## Avertissement

DRAFT est indépendant et n'est ni approuvé ni sponsorisé par Riot Games. Le site ne constitue pas une approbation des fonctionnalités en cours d'expérimentation.
