# Comptabilité Mensuelle

Application web légère de gestion de comptabilité mensuelle : revenus, dépenses, solde et graphiques de consommation. Fonctionne entièrement dans le navigateur, sans serveur ni inscription — les données sont stockées localement sur l'appareil.

> Page en ligne : https://wre-panda-cyber.github.io/Mistral-code/

## Fonctionnalités

- **Gestion mensuelle** : saisie des transactions (revenus / dépenses), consultation du solde par mois, navigation entre les mois.
- **Catégorisation** : chaque transaction peut être rattachée à une catégorie (Logement, Énergie, Courses, Transport, Sorties, Salaire, etc.).
- **Graphiques de consommation** :
  - barres — revenus vs dépenses du mois
  - anneau (donut) — répartition des dépenses par catégorie
  - courbe — évolution du solde sur 6 mois
- **Import de documents** (PDF, JPG, PNG, captures d'écran iPhone) avec extraction automatique du texte :
  - PDF textuel → extraction via pdf.js
  - PDF scanné / image → OCR via Tesseract.js (français)
  - **Auto-remplissage** des lignes à partir d'un relevé bancaire (tableau Date · Libellé · Débit · Crédit · Solde) ; détection débit = dépense, crédit = revenu, solde ignoré.
- **Import mobile-first** : gros boutons Photo (caméra) et Fichier, optimisés pour iPhone Safari.
- **Export PDF** du mois courant (jsPDF + autotable).

## Utilisation

1. Choisir le mois en haut de page.
2. Saisir une transaction via le formulaire (date, description, catégorie, type, montant) puis **Ajouter**.
3. Pour importer un relevé : cliquer **Photo** (caméra iPhone) ou **Fichier** et sélectionner un PDF / JPG / PNG. Les transactions détectées sont ajoutées automatiquement au tableau.
4. Consulter les graphiques et le solde en bas de page.
5. Exporter le mois en PDF via le bouton **Exporter PDF**.

Les données sont sauvegardées dans le `localStorage` du navigateur (`compta_transactions_v1`, `compta_documents_v1`).

## Fonctionnement technique

- **Fichier unique** `index.html` : HTML, CSS et JavaScript en un seul fichier, sans build ni serveur.
- **Bibliothèques (CDN)** :
  - [Chart.js 4.4.1](https://www.chartjs.org/) — graphiques
  - [pdf.js 3.11.174](https://mozilla.github.io/pdf.js/) — extraction de texte PDF
  - [Tesseract.js 5.1.0](https://tesseract.projectnaptha.com/) — OCR images (français)
  - [jsPDF 2.5.1](https://github.com/parallax/jsPDF) + [jspdf-autotable 3.8.2](https://github.com/simonbengtsson/jsPDF-AutoTable) — export PDF
- **Stockage** : `localStorage` uniquement (aucune donnée envoyée sur un serveur).
- **Extraction des relevés** : reconstruction des lignes par coordonnée Y (tolérance ±5 px) puis parsing des colonnes Débit / Crédit / Solde, avec gestion des séparateurs de milliers et des décimales à point ou virgule.

## Déploiement

L'application est servie via GitHub Pages à partir de la branche `main` :
https://wre-panda-cyber.github.io/Mistral-code/

Aucune installation, aucune dépendance serveur. Ouvrir l'URL dans un navigateur, sur ordinateur ou mobile.

## Limites

- Le premier lancement de l'OCR peut prendre 20–30 s (téléchargement du modèle Tesseract).
- Les PDF de plus de 15 Mo ne sont pas pris en charge ; pour un gros relevé scanné, privilégier une photo ou un PDF réduit.
- La qualité de l'extraction dépend du PDF bancaire ; si l'auto-remplissage échoue, le panneau « Détails de l'extraction » affiche le texte brut pour calibrage.
