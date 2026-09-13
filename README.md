# Bollard Trainer

🔗 [Open App](https://tgm-hub.github.io/bollards-trainer)
Entraînement à la reconnaissance des bornes de signalisation pour GeoGuessr.
949 bornes, 109 pays.

## Lancer

Double-clic sur `index.html`. Pas de serveur, pas de build.

## Contenu

```
index.html          l'app (autonome, tout inline)
bollards.data.js    le dataset, chargé automatiquement au démarrage
bollards.json       le même dataset en JSON standard
images/             les 949 photos
scrape-geohints.js  reconstruit le dataset depuis zéro (voir plus bas)
```

## Ce que fait l'app

- **QCM avec distracteurs de la même famille.** Les mauvaises réponses partagent
  les couleurs et matériaux du bon pays (similarité de Jaccard sur les tags +
  bonus même continent). Un QCM aléatoire n'apprend rien ; celui-ci force sur
  les vraies frontières de décision.
- **Répétition espacée indexée sur le pays, pas sur la photo.** Boîtes de
  Leitner, et à chaque question une photo au hasard parmi celles du pays — on
  apprend la borne, pas l'image.
- **Matrice de confusion.** L'onglet Stats liste ce que tu confonds avec quoi.
  C'est la liste de révision, générée toute seule.

Clavier : `1`-`4` pour répondre, `Espace` pour enchaîner.

## Réglages utiles

Les « Rare » sont décochés par défaut : ils pèsent 493 des 949 entrées mais ne
se croisent quasiment jamais en partie. Restent 456 photos / 79 pays.

Sur ces 456, **170 sont américaines** (37 %). Si tu ne joues pas beaucoup de
cartes US, décoche l'Amérique du Nord : ~286 photos bien mieux réparties, et la
répétition espacée arrête de saturer sur un seul pays.

## Reconstruire le dataset

Le dataset vient de [geohints.com/meta/bollards](https://geohints.com/meta/bollards).
Pour le régénérer (nouvelles entrées ajoutées par GeoHints, par exemple) :

1. ouvrir la page, console (F12), coller `scrape-geohints.js` ;
2. il extrait les entrées puis rejoue les 24 filtres du site pour récupérer les
   tags (couleur, matériau, rareté, continent), 400 ms entre chaque requête ;
3. il télécharge `bollards.json`, `bollards.data.js` et `images.txt`.

Pour les images : `mkdir images && wget -i images.txt -P images --wait=0.3 --random-wait`
— ou simplement « Enregistrer la page complète » depuis Chrome, qui produit le
dossier `images/` avec les bons noms.

Note : le filtre `Type` (Bollard / Barricade / Snowpole…) est cassé côté
GeoHints — il renvoie les 949 entrées quelle que soit la valeur. Le script
l'ignore volontairement, sinon les tags seraient faux.

## Crédit

Données de localisation curées par [GeoHints](https://geohints.com/meta/bollards).
Les photos sont des captures Google Street View. Usage strictement personnel :
ce repo reste privé et n'est pas déployé.
