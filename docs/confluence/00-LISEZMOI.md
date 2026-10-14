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

- **Texte** : dans l'éditeur, `/markdown` (Cloud) ou *Insérer > Balisage > Markdown* (Data Center), puis coller le contenu du fichier `.md`. Les tableaux et les blocs de code sont convertis.
- **Diagrammes** : insérer les PNG du dossier `img/` à la place des lignes `![…](img/…)`. Les sources `.mmd` (Mermaid) permettent de les modifier : avec une appli Mermaid pour Confluence, ou via mermaid.live, puis réexport en PNG.
- **Liens entre pages** : remplacer les liens `xx-….md` par les liens Confluence une fois les pages créées.

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
