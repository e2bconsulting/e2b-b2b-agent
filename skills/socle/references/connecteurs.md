# Connecteurs : opérations abstraites et correspondance

Les skills parlent d'**opérations** ; ce fichier dit quel outil du connecteur choisi (`config.md`) l'exécute. Si un outil n'existe pas sous le nom indiqué, utiliser l'outil équivalent du connecteur ; si aucun ne convient, le dire dans le récapitulatif au lieu de contourner.

## 1. EMAIL (obligatoire)
| Opération | Gmail | Outlook (Microsoft 365) |
|---|---|---|
| `email.lire_recents` | `search_threads` puis `get_thread` | rechercher les messages de la boîte de réception, lire la conversation complète |
| `email.lire_fil` | `get_thread` (format texte) | lire la conversation |
| `email.identifier_boite` | expéditeur des derniers envois (`in:sent`) | adresse du compte connecté / éléments envoyés |
| `email.creer_brouillon` | `create_draft` avec `replyToMessageId` | créer un brouillon de réponse dans le dossier Brouillons |
| Interdit | envoi, réponse directe, transfert, suppression, archivage, libellés | envoi, réponse directe, transfert, suppression, déplacement |

Un brouillon, jamais un envoi : si l'outil de brouillon n'existe pas, s'arrêter et signaler.

## 2. CRM (optionnel) — contexte client et engagement
Le CRM sert à **comprendre** le client avant d'écrire, pas à stocker les fiches de relance (c'est le rôle de SUIVI). Lecture seule par défaut.

| Opération | HubSpot | Brevo |
|---|---|---|
| `crm.chercher_contact` | recherche du contact par email | `contacts_get_contact_info` |
| `crm.contexte` | étape du cycle de vie, affaires ouvertes, dernières notes, dernier contact | listes d'appartenance, attributs, statistiques d'engagement (ouvertures, clics) |
| `crm.statut_desinscription` | propriété de désabonnement / statut du contact | contact désinscrit ou sur liste noire |
| `crm.notes_client` | lire les notes du contact et de l'affaire | lire les attributs et les notes du contact |
| `crm.noter` (facultatif, si autorisé dans `config.md`) | ajouter une note sur le contact ou l'affaire | non disponible : ne rien écrire |

Règles CRM :
- Contact désinscrit ou sur liste noire → traiter comme **refus définitif** (aucune relance), même si le fil Email est silencieux.
- Ne jamais créer ni lancer de campagne email ou SMS, ni modifier des listes d'envoi.
- Une affaire déjà ouverte dans HubSpot pour le même besoin → la mentionner dans `Tâche liée`, ne pas la dupliquer.
- Aucune donnée du CRM n'est recopiée dans un brouillon (engagement, scoring) : elle sert uniquement à choisir l'angle.

## 3. SUIVI (obligatoire) — base où l'agent écrit ses fiches
| Opération | ClickUp | Notion | Airtable | Drive (Google Sheet) |
|---|---|---|---|---|
| `suivi.lister_dues` | filtrer les tâches de la liste par échéance ≤ aujourd'hui et statut | interroger la base : `Prochaine relance` ≤ aujourd'hui, statut ouvert | enregistrements dont `Prochaine relance` ≤ aujourd'hui | lire la feuille puis filtrer les lignes |
| `suivi.chercher` (dédoublonnage) | recherche dans la liste, tâches closes incluses, toutes pages | interroger la base par nom / fil, fiches closes incluses | recherche par nom / fil, fiches closes incluses | chercher dans la feuille, lignes closes incluses |
| `suivi.statut` | lire le type du statut (`Done` / `Closed`), à défaut `date_closed` | lire la propriété `Statut` | lire le champ `Statut` | lire la colonne `Statut` |
| `suivi.creer` | créer une tâche | créer une page dans la base | créer un enregistrement | ajouter une ligne |
| `suivi.maj` | modifier la tâche | modifier les propriétés de la page | modifier l'enregistrement | modifier la ligne |
| `suivi.journal` | commentaire sur la tâche | commentaire sur la page | note ou champ `Journal` | colonne `Journal` (ajout daté) |

Limite Drive : un tableur est moins adapté qu'une vraie base (pas de filtre serveur, risque d'écritures concurrentes si plusieurs personnes passent en même temps). À réserver aux petites équipes ; dans ce cas, relire la feuille juste avant chaque écriture.

## 4. FACTURATION (optionnel) — état des factures, lecture seule
| Opération | Swiver |
|---|---|
| `facturation.etat_client` | état des factures et règlements du client : à jour, en retard (nombre de jours), aucune facture |

Règles : aucun montant n'est jamais recopié dans un brouillon ni dans un dossier ; seul le statut compte. Un client en retard de paiement n'est pas relancé commercialement avant concertation avec la finance. Sans connecteur de facturation, la ligne `Finance` du dossier reste `non disponible`.

## 5. AGENDA, FICHIERS (optionnels)
- `agenda.lire` : Google Agenda ou Outlook, **lecture seule**, pour les dates de sessions et les créneaux proposés. Sans agenda, ne citer aucune date.
- `fichiers.lire` : Drive ou OneDrive, **lecture seule**, pour le kit d'offre. L'agent n'attache rien : il écrit `[PIÈCES JOINTES : …]`.

## 6. Ordre d'utilisation dans un passage
1. `config.md` puis contrôles de démarrage.
2. `email.identifier_boite`, `email.lire_recents`.
3. Pour chaque demande : `suivi.chercher` (dédoublonnage) puis, si CRM actif, `crm.chercher_contact` et `crm.statut_desinscription`.
4. `suivi.creer` ou `suivi.maj`, puis `email.creer_brouillon`, puis `suivi.maj` et `suivi.journal`.
5. `suivi.lister_dues` pour les relances.
