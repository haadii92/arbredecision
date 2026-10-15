# Modifications de la doc Confluence : choix de base de données

## Arborescence proposée

```
Choisir sa base de données              ← nouvelle page parente (01-choisir-sa-base.md)
├── Nouveau projet : choisir son moteur ← nouvelle page (02-nouveau-projet.md)
├── Migration : choisir sa méthode      ← remplace la page actuelle (03-migration.md)
├── Runbooks techniques                 ← existant, + section MongoDB (04-runbook-mongodb.md)
└── Suivi des migrations                ← existant, + colonne Techno
```

## Mise en page dans Confluence

### Option 0 : copier-coller depuis le navigateur (le plus simple sous Confluence Cloud)

1. Dézipper l'archive, ouvrir `html/<page>.html` dans le navigateur (double-clic).
2. Ctrl + A, Ctrl + C dans le navigateur.
3. Ctrl + V dans le corps de la page Confluence : titres, tableaux, listes et blocs de code gardent leur mise en forme.
4. Si le diagramme ne passe pas au collage : le supprimer et le réinsérer avec l'icône image de la barre d'outils (fichier dans `img/`).

### Option 1 : wiki markup

Le dossier `wiki/` contient chaque page convertie en wiki markup Confluence.

1. Créer la page avec le titre indiqué ci-dessous (le titre n'est pas dans le fichier).
2. Joindre à la page le PNG correspondant de `img/` (*Joindre des fichiers*), avant de coller : le markup y fait référence par son nom.
3. Dans l'éditeur : *Insérer > Balisage* (*Markup*), choisir **Confluence wiki**, coller le contenu du `.txt`, puis *Insérer*.

| Fichier | Titre de la page | Image à joindre |
|---|---|---|
| `wiki/01-choisir-sa-base.txt` | Choisir sa base de données | `arbre-global.png` |
| `wiki/02-nouveau-projet.txt` | Nouveau projet : choisir son moteur | `arbre-nouveau-projet.png` |
| `wiki/03-migration.txt` | Migration : choisir sa méthode | `arbre-migration.png` |
| `wiki/04-runbook-mongodb.txt` | À coller dans la page Runbooks techniques | — |

Les liens entre pages pointent vers ces titres : ils fonctionnent si les pages portent exactement ces noms, dans le même espace.

### Option 2 : Markdown (validé sous Confluence Cloud)

1. Ouvrir le `.md` dans le **Bloc-notes**, pas dans VS Code : VS Code copie le texte en mode code, et Confluence le colle dans un bloc de code.
2. Ctrl + A, Ctrl + C, puis Ctrl + V dans le corps de la page : Confluence convertit le Markdown.
3. Supprimer le titre `#` de la première ligne : il doublonne le titre de la page.
4. Remplacer chaque ligne `![…](img/…)` par l'image, avec l'icône image de la barre d'outils (fichier dans `img/`).
5. Une fois toutes les pages créées, refaire les liens `xx-….md` : sélectionner le texte, Ctrl + K, chercher la page Confluence.

## Ce qui change par rapport à la doc actuelle

**Nouveau périmètre**

- Page d'entrée commune : nouveau projet ou migration.
- Nouvelle page **Nouveau projet** : choix du moteur (éditeur, modèle de données, écosystème), architecture selon la criticité, questions de cadrage, déroulé.
- **MongoDB** ajouté partout : version cible, seuil de dump, méthode A, nouvelle méthode **F — Extension du replica set**, questions DBA, contrôles, runbook.
- Tableau des versions cibles déplacé sur la page d'entrée, commun aux deux cas.

**Migration : corrections de l'arbre**

- Branches sans issue fermées : PostgreSQL avec tables sans PK (ajouter les PK puis D) ou avec des large objects (A) ; MariaDB avec un saut de version non validé (A) ; MongoDB shardé (cas DBA).
- Sources les plus anciennes notées (PostgreSQL 10, SQL Server 2016) : les conditions « 10 ou plus » et « 2016 ou plus » sont toujours remplies et sortent de l'arbre.
- Source en PostgreSQL 10 : les `TRUNCATE` ne sont pas répliqués en méthode D.
- Source SQL Server 2016 : niveau de compatibilité 130 conservé à la bascule.
- Critère « base critique » : proposition provisoire en attendant la liste (coupure acceptable < 1 h ou appli 24 h / 24).
- Formule d'estimation de durée à partir du débit indicatif.

**Migration : points d'attention ajoutés**

- D : rétention des WAL par le slot de réplication, gel des changements de schéma, `REPLICA IDENTITY FULL` en dépannage.
- E : rétention des binlogs, tables MyISAM, chargement initial par dump logique (`mariadb-backup` exclu, c'est une copie physique).
- Retour arrière : F rejoint D comme méthode réversible, jusqu'au retrait des anciens membres.
- Jour J : contrôles adaptés à MongoDB (documents, index), retrait de Commvault et Zabbix sur l'ancien serveur.
- Alias DNS : enregistrement SRV pour MongoDB.

**À trancher (nouveaux points)**

- Version et édition MongoDB cibles, présence de MongoDB shardé.
- Débits indicatifs MongoDB à mesurer.
- Standard de haute disponibilité MariaDB pour les nouveaux projets.
- PostgreSQL comme moteur relationnel par défaut pour les nouveaux projets (proposition).
- Cibles PostgreSQL 17 / SQL Server 2022 : à conserver, ou à remplacer pour les nouveaux projets par les versions plus récentes disponibles (PostgreSQL 18, SQL Server 2025).
