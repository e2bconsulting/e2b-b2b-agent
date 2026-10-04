---
name: apres-vente
description: Gère l'après-vente en brouillons — confirmation de commande, demande de validation ou d'attestation après livraison, relance de facture impayée — et met à jour la fiche CRM. Utiliser quand l'utilisateur dit « le client a confirmé », « relance la facture de X », ou quand une fiche passe en Gagné.
---

# Après-vente

Lire `regles.md`, `equipe.md`, `ton-signatures.md`, `relances.md`, `crm.md` (dans `../socle/references/`). Brouillon seulement si le récepteur principal est `BOITE`.

## A. Confirmation de commande
Remerciement, rappel de l'offre et des dates (calendrier), prochaines étapes, informations à fournir par le client. Fiche : `Gagné`, échéance = lendemain de la livraison. Assigner la finance.

## B. Après livraison
Remerciement, questionnaire de satisfaction, demande de validation ou d'attestation de bonne exécution. Relance à J+5 ouvrés, puis J+15. Document reçu → `Action humaine : facturer`.

## C. Facture impayée
Uniquement à partir d'une information fournie (fil de facture, liste de facturation, indication de l'utilisateur) : ne jamais supposer un impayé. « Sauf erreur de notre part, nous n'avons pas encore reçu le règlement de la facture n° <n> du <date>. Pourriez-vous nous indiquer la date de paiement prévue ? » Aucun montant. Cadence J+30, J+45, J+60 (appel téléphonique). Paiement confirmé → `complete`.

## D. Nouvelle opportunité
21 jours après la livraison, fiche `Gagné` : proposer dans le récapitulatif (pas de brouillon automatique) une suite ou une montée en gamme.
