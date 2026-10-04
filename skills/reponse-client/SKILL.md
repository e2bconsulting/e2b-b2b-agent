---
name: reponse-client
description: Rédige un brouillon Gmail de réponse à une demande commerciale B2B (offre, information, qualification d'un lead, dossier d'appel d'offres), sans prix, depuis la boîte du récepteur principal, puis met à jour la fiche CRM. Utiliser quand l'utilisateur dit « réponds à cette demande », « prépare l'offre pour ce client », ou après tri-demandes.
---

# Réponse à une demande commerciale

Lire `../socle/references/regles.md`, `equipe.md`, `catalogue.md`, `ton-signatures.md`, `kit-offre.md`, `faq-objections.md`, `crm.md`.

1. Vérifier que le récepteur principal est `BOITE`, sinon s'arrêter (voir `equipe.md`, étape 2).
2. Relire le fil complet et lister **toutes les questions** posées par le client.
3. Choisir le modèle de `ton-signatures.md` (offre sur mesure, session à dates fixes, qualification, accusé de réception en retard).
4. Rédiger : intitulé canonique, réponses point par point depuis le catalogue et la FAQ, dates lues dans le calendrier le jour même, `[OFFRE FINANCIÈRE À JOINDRE]` et `[PIÈCES JOINTES : …]` selon le kit. Si une information manque : formulation prudente + `À compléter`.
5. `create_draft` avec `replyToMessageId`, copies selon `equipe.md`, signature du collaborateur.
6. Mettre à jour la fiche : `Brouillon : prêt`, `Offre financière à chiffrer`, prochaine relance (voir `relances.md`, ex. offre sans retour J+4), journal.
7. Contrôle : aucun montant, aucune mention d'un autre client, aucun crochet résiduel.
