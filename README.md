# e2b-b2b-agent

Un agent de **relation client B2B** pour Claude (Cowork / Claude Code), conçu par E2B Consulting & Training à partir d'un cas réel : répondre aux demandes entrantes, ne jamais laisser une demande sans suite, et relancer au bon moment.

> L'agent **prépare**, l'humain **valide et envoie**. Aucun email n'est jamais envoyé automatiquement.

## Ce qu'il fait
1. **Trie** la boîte Gmail de chaque collaborateur et écarte le bruit.
2. **Identifie le récepteur principal** de chaque demande (destinataire, copie, transfert interne, adresse générique) et ne rédige que depuis sa boîte.
3. **Trace** chaque demande dans une fiche CRM normalisée (exemple fourni pour ClickUp) : statut, intention, signal client, prochaine relance.
4. **Rédige des brouillons Gmail** dans le ton de l'entreprise, sans aucun prix par défaut.
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
| `revue-hebdo` | Revue du pipeline |
| `socle` | Règles et base de connaissances (à personnaliser) |

## Mise en route (30 minutes)
1. Installer le plugin dans Claude (Cowork ou Claude Code).
2. Chaque utilisateur connecte **sa propre** boîte Gmail, Google Calendar, Google Drive et votre CRM (ClickUp dans l'exemple).
3. Personnaliser `skills/socle/references/` : `equipe.md` (adresses, rôles), `catalogue.md` (offres), `ton-signatures.md`, `faq-objections.md`, `kit-offre.md`, et les paramètres de `crm.md` / `relances.md`.
4. Lancer un premier passage à blanc sur une boîte et relire les brouillons.
5. Planifier le passage quotidien (ex. 9h00, jours ouvrés) : « Lance tour-quotidien ».

## Principes de conception
- **Brouillons uniquement** : droits d'écriture Gmail limités à `create_draft`.
- **Pas de prix** : l'agent insère `[OFFRE FINANCIÈRE À JOINDRE]`. Paramétrable.
- **Une seule source de vérité** pour les connaissances : le dossier `references/`. L'agent n'invente rien et signale `À compléter`.
- **Le CRM est la mémoire partagée** : fiche structurée, date d'échéance = date de relance, journal des actions.
- **Le client fixe le rythme** : la relance se déduit de la dernière phrase du client.
- **Protection des données** : pas de téléphone ni de montant dans les titres CRM, jamais d'information d'un client citée à un autre.

## Retour d'expérience (test réel)
Lacunes trouvées au premier test et à traiter dans votre version : qui signe quand le client écrit à un dirigeant en tant qu'expert ; détecter qu'une demande a déjà été transférée en interne ; dédoublonner aussi dans les autres listes et les tâches closes ; ne pas recopier de données personnelles dans les titres existants.

## Licence
MIT. © E2B Consulting & Training.
