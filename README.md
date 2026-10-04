# e2b-b2b-agent

Un agent de **relation client B2B** pour Claude (Cowork / Claude Code), conçu par E2B Consulting & Training à partir d'un cas réel : répondre aux demandes entrantes, ne jamais laisser une demande sans suite, et relancer au bon moment.

> L'agent **prépare**, l'humain **valide et envoie**. Aucun email n'est jamais envoyé automatiquement.

## Ce qu'il fait
1. **Trie** la boîte email (Gmail ou Outlook) de chaque collaborateur et écarte le bruit.
2. **Identifie le récepteur principal** de chaque demande (destinataire, copie, transfert interne, adresse générique) et ne rédige que depuis sa boîte.
3. **Trace** chaque demande dans une fiche de suivi normalisée, écrite dans la base de votre choix (ClickUp, Notion, Airtable ou Google Sheet) : statut, intention, signal client, prochaine relance.
4. **Rédige des brouillons email** dans le ton de l'entreprise, sans aucun prix par défaut.
5. **Relance de façon contextuelle** : J+3/J+10 sans réponse, « revenez en janvier », « dans 2 mois », « pas de budget cette année », indécis, offre sans retour, dormants.
6. **Revue hebdomadaire** du pipeline.

## Architecture
| Skill | Rôle |
|---|---|
| `tour-quotidien` | Orchestre un passage complet (à planifier chaque matin) |
| `tri-demandes` | Récepteur principal, classement, fiche CRM |
| `reponse-client` | Brouillon de réponse à une demande commerciale |
| `apres-vente` | Confirmation, livrables, facture impayée |
| `moteur-relances` | Relances dues et recalcul des dates |
| `dossier-client` | Veille 2 fois par semaine : dossier de synthèse sourcé par client, signaux et vigilances |
| `revue-hebdo` | Revue du pipeline |
| `configuration` | Choix des connecteurs, préparation de la base de suivi, test à blanc |
| `socle` | Règles, connecteurs et base de connaissances (à personnaliser) |

## Connecteurs pris en charge
| Couche | Options | Rôle | Droits |
|---|---|---|---|
| Email | Gmail, Outlook | Lire les demandes, créer des brouillons | Brouillons uniquement |
| CRM (optionnel) | HubSpot, Brevo | Contexte client, engagement, désinscriptions | Lecture seule |
| Base de suivi | ClickUp, Notion, Airtable, Google Drive (Sheet) | Fiches, échéances de relance, journal | Écriture sur la base configurée |
| Facturation (optionnel) | Swiver | Retards de paiement (statut sans montant) | Lecture seule |
| Agenda (optionnel) | Google Agenda, Outlook | Dates de sessions et créneaux | Lecture seule |
| Fichiers (optionnel) | Drive, OneDrive | Kit d'offre | Lecture seule |

Le choix se fait dans `skills/socle/references/config.md` ; la correspondance opération → outil est dans `connecteurs.md`. Ajouter un connecteur = ajouter une colonne dans ce fichier.

## Mise en route (30 minutes)
1. Installer le plugin dans Claude (Cowork ou Claude Code).
2. Chaque utilisateur connecte **sa propre** boîte email, plus la base de suivi, et (facultatif) le CRM, l'agenda et les fichiers.
3. Lancer le skill `configuration` : il choisit les connecteurs avec vous, prépare la base de suivi et fait un test à blanc.
4. Personnaliser `skills/socle/references/` : `equipe.md` (adresses, rôles), `catalogue.md` (offres), `ton-signatures.md`, `faq-objections.md`, `kit-offre.md`, et les paramètres de `relances.md`.
5. Relire les premiers brouillons, puis planifier le passage quotidien (ex. 9h00, jours ouvrés) : « Lance tour-quotidien ».

## Principes de conception
- **Brouillons uniquement** : jamais d'envoi, quel que soit le connecteur email.
- **CRM en lecture seule** : il éclaire la réponse (cycle de vie, affaire ouverte, désinscription) sans jamais lancer de campagne ; un contact désinscrit n'est jamais relancé.
- **Connecteurs interchangeables** : les skills parlent d'opérations abstraites, pas d'outils.
- **Pas de prix** : l'agent insère `[OFFRE FINANCIÈRE À JOINDRE]`. Paramétrable.
- **Une seule source de vérité** pour les connaissances : le dossier `references/`. L'agent n'invente rien et signale `À compléter`.
- **La base de suivi est la mémoire partagée** : fiche structurée, date d'échéance = date de relance, journal des actions.
- **Le client fixe le rythme** : la relance se déduit de la dernière phrase du client.
- **Protection des données** : pas de téléphone ni de montant dans les titres CRM, jamais d'information d'un client citée à un autre.

## Retour d'expérience (test réel)
Lacunes trouvées au premier test et à traiter dans votre version : qui signe quand le client écrit à un dirigeant en tant qu'expert ; détecter qu'une demande a déjà été transférée en interne ; dédoublonner aussi dans les autres listes et les tâches closes ; ne pas recopier de données personnelles dans les titres existants .

## Licence
MIT. © E2B Consulting & Training.

## Veille : le dossier client
`dossier-client` tourne deux fois par semaine (par exemple lundi et jeudi, avant le passage quotidien). Pour les clients en veille, dormants ou avec une offre en attente, il lit le suivi, les notes du CRM, les emails, la facturation et l'agenda, puis écrit un dossier de 25 lignes maximum, **sourcé et daté**, avec un angle de relance recommandé et les points de vigilance (impayé, désinscription). Les autres skills s'en servent pour choisir leur angle, sans jamais le citer au client.
