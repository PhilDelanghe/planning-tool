# Planning Tool - Mode d'emploi

## Démarrage rapide

**Double-clic sur `planning.html`** et c'est parti. Rien à installer.

L'appli fonctionne en deux modes :

| Mode | Comment | Avantage |
|------|---------|----------|
| **Autonome** | Double-clic sur le fichier HTML | Zéro installation, marche partout |
| **Serveur** | Dossier partagé (SharePoint, OneDrive, NAS) ou `python3 -m http.server 8765` | Le HTML lit `planning.md` automatiquement, plusieurs personnes sur les mêmes fichiers |

En mode autonome, les données sont embarquées dans le HTML. En mode serveur, le HTML lit le fichier `planning.md` séparé (éditable dans n'importe quel éditeur de texte).

## Fichier source (planning.md)

Le planning est défini dans un fichier Markdown lisible et éditable à la main. C'est la spec du projet.

### Structure

```markdown
# Nom du planning

## Paramètres
- Date de départ: 2026-03-09
- Durée: 15 semaines
- Client: Client
- Prestataire: Prestataire

## Légende
- client: #FFD966 "Actions Client" acteur="Client"
- presta: #4DD9C0 "Actions Prestataire" acteur="Prestataire"
- sync: #B266FF "Temps synchrones" acteur="Client/Prestataire"
- livraison: #93C47D "Livraisons" acteur="Prestataire"

## Planning

### Nom de la phase
- Nom de la tâche | code_légende
- Autre tâche | code_légende | 10/03 | 3j
```

### Conventions

- `## Paramètres` : date de début, durée en semaines, noms du client et du prestataire
- `## Légende` : chaque entrée a un code, une couleur hex, un label descriptif, et le nom de l'acteur affiché dans la colonne Acteurs
- `### Titre` : crée une ligne de phase (bandeau gris)
- `- Tâche | code` : crée une tâche. Le code correspond à une entrée de la légende
- `- Tâche | code | JJ/MM | Nj` : tâche avec date de début et durée (renseignées automatiquement à la sauvegarde)

Les tâches sans date/durée apparaissent vides dans le planning (pas encore planifiées).

### Adaptation à un nouveau projet

1. Dupliquer `planning.md` (ou partir de zéro)
2. Modifier le titre, les paramètres (date, durée, client, prestataire)
3. Adapter la légende si les acteurs changent (codes, couleurs, noms)
4. Lister les phases (`### ...`) et les tâches (`- ... | code`)
5. Ouvrir l'appli, peindre le planning, sauvegarder

Les codes de la légende sont libres. On peut en ajouter ou en retirer selon le projet.

## Utilisation de l'appli

### Peindre des cellules

- **Clic gauche** sur une cellule vide : la remplit avec la couleur de l'acteur de la ligne
- **Clic gauche sur une cellule remplie** : l'efface (toggle)
- **Clic + glisser** : peint plusieurs cellules d'affilée sur la même ligne

Les weekends et jours fériés sont automatiquement sautés (pas de peinture possible).

### Menu contextuel (clic droit sur une cellule)

- **Remplir + durée** : remplit N jours ouvrés à partir de la cellule cliquée (1, 2, 3, 4, 5, 10 ou 15 jours)
- **Effacer cette cellule** : vide une cellule
- **Effacer toute la ligne** : vide toutes les cellules de la tâche
- **Décaler cette tâche** : décale les cellules de la tâche de N jours ouvrés (positif = droite, négatif = gauche)
- **Décaler + suivantes** : décale cette tâche et toutes celles qui suivent dans le planning (décalage en cascade)

Les décalages sautent automatiquement les weekends et les jours fériés.

### Colonne "Jours"

Entre Tâches et Acteurs. Affiche le nombre de jours ouvrés remplis par tâche. Se met à jour en temps réel quand on peint ou efface.

### Totaux

En bas du tableau : Total Prestataire et Total Client (les noms viennent des paramètres). Mis à jour en temps réel.

### Jours fériés français

Calculés automatiquement pour chaque année (y compris les fêtes mobiles : lundi de Pâques, Ascension, lundi de Pentecôte). Ils apparaissent en **rose clair** dans le calendrier avec le nom du férié au survol de la souris. Ils sont bloqués comme les weekends : pas de peinture, pas de comptage, sautés par les décalages.

### Paramètres (barre du haut)

- **Début** : date de démarrage du planning
- **Semaines** : nombre de semaines affichées
- **Appliquer** : recalcule la grille avec les nouveaux paramètres

## Sauvegarder / Charger / Export / PDF

### Sauvegarder

Exporte un fichier `.md` versionné (ex: `planning_v3_2026-04-16.md`).

Ce fichier contient :
- La structure complète du planning (phases, tâches, acteurs)
- Les dates de début et durées de chaque tâche (calculées depuis les cellules peintes)

Le fichier est lisible tel quel dans n'importe quel éditeur de texte. Les versions s'accumulent dans le dossier : on peut toujours revenir à une version antérieure en la rechargeant.

Le bloc embarqué dans le HTML est aussi mis à jour, pour que le mode autonome (double-clic) retrouve les dernières données.

### Charger

Ouvre un sélecteur de fichier. Accepte les fichiers `.md` (spec) ou `.html` (version exportée). Remplace le planning en cours.

### Export client

Génère un fichier HTML autonome en **lecture seule**. Le destinataire double-clique pour voir le planning, peut l'imprimer, mais ne peut pas le modifier. Les données source (Markdown) sont embarquées dans le fichier pour permettre un réimport ultérieur via "Charger".

### PDF / Impression

Ouvre la boîte d'impression du navigateur. Le planning est automatiquement découpé en pages de 5 semaines, avec les colonnes de gauche (Tâches, Jours, Acteurs) répétées sur chaque page. Le zoom s'ajuste automatiquement au nombre de lignes. L'orientation paysage est forcée.

## Résumé des fichiers

| Fichier | Rôle |
|---------|------|
| `planning.html` | L'appli (code + données embarquées). Double-clic pour ouvrir. |
| `planning.md` | La spec du projet (éditable). Lue automatiquement en mode serveur. |
| `planning_vN_date.md` | Versions sauvegardées du planning (une par sauvegarde). |
| `README.md` | Ce fichier. |
