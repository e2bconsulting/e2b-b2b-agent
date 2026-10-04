# Configuration de l'agent (à remplir une fois)

Les skills lisent ce fichier au début de chaque passage. Choisissez un connecteur par couche ; les autres blocs sont ignorés.

```
EMAIL   = gmail            # gmail | outlook
CRM     = none             # none | hubspot | brevo
SUIVI   = clickup          # clickup | notion | airtable | drive
AGENDA  = google           # google | outlook | none   (dates des sessions et créneaux)
FICHIERS= drive            # drive | onedrive | none   (kit d'offre, pièces jointes)
FACTURATION = none         # swiver | none   (état des factures et règlements, lecture seule)
WEB     = non              # oui | non        (actualité publique de l'entreprise dans les dossiers clients)
DOSSIERS= suivi            # emplacement des dossiers clients : suivi (voir `suivi.md` et `dossier-client`)
LANGUE  = fr
```

## Paramètres par connecteur
| Connecteur | À renseigner |
|---|---|
| ClickUp | identifiant du workspace, identifiant de la liste de suivi, identifiants des membres |
| Notion | identifiant de la base de données de suivi (propriétés : voir `suivi.md`) |
| Airtable | identifiant de la base et de la table de suivi |
| Drive | identifiant du Google Sheet de suivi (une ligne par fiche) |
| HubSpot | propriétaire par défaut, pipeline et étapes utilisés |
| Brevo | liste(s) à consulter pour l'engagement, notes client |
| Swiver | société(s) à consulter pour les factures et règlements |

## Si le fichier n'est pas rempli
Demander à l'utilisateur, une seule fois et en une question, quels connecteurs utiliser (proposer ceux qui sont réellement connectés), puis afficher le bloc rempli pour qu'il l'enregistre ici.

## Contrôles au démarrage
1. Le connecteur EMAIL choisi est connecté et répond (lecture d'un fil récent).
2. Le connecteur SUIVI choisi est connecté et la base cible est accessible.
3. En cas d'échec d'une couche : continuer avec ce qui fonctionne, ne rien inventer, et signaler en tête du récapitulatif.
