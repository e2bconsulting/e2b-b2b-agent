---
name: moteur-relances
description: Exécute les relances dues (prospects, offres sans retour, rappels demandés à 1, 2 ou 6 mois, indécis, dormants) depuis la base de suivi, vérifie Gmail, rédige les brouillons depuis la bonne boîte et recalcule la prochaine date selon ce que dit le client. Utiliser quand l'utilisateur dit « quelles relances aujourd'hui », « lance les relances », ou comme étape de tour-quotidien.
---

# Moteur de relances

Lire `../socle/references/relances.md` (règles complètes), `equipe.md`, `ton-signatures.md`, `suivi.md`.

## 1. Relances dues
Identifier `BOITE`. Lister les fiches `to do` et `in progress` dont l'échéance ≤ aujourd'hui et assignées à la personne (paginer). Ignorer celles dont le récepteur principal est un autre collaborateur, sauf `Boîte de réponse` explicite.

## 2. Pour chaque relance due
1. Contrôle de date manuelle (`suivi.md`).
2. Lire `Angle recommandé` et `Points de vigilance` du dossier client (`dossier-client`) : retard de paiement → relance suspendue et signalée ; contenu jamais cité dans le brouillon. Relire le fil (`email.lire_fil`) et, si le CRM est actif, vérifier `crm.statut_desinscription`.
3. Le client a répondu depuis la dernière action interne → ne pas relancer : détecter le nouveau signal, mettre à jour fiche et échéance, enchaîner sur `reponse-client` si une réponse est attendue.
4. Un humain a déjà relancé → mettre à jour `Dernier contact` et recalculer.
5. Sinon → brouillon de relance dans le fil (5 à 8 lignes, une information nouvelle, une question fermée ou un créneau), `Nb relances` +1, `Brouillon : prêt`, prochaine date calculée et ajustée, journal.

## 3. Exemples
- « Revenez vers moi en janvier » → premier jour autorisé de janvier ; `En veille`.
- « Dans 2 mois » → date moins 3 jours ouvrés, ajustée.
- « Je dois en discuter en interne » → J+30, J+60, J+90, J+180, angle différent à chaque fois.
- « Pas de budget cette année » → début d'exercice suivant.
- « Ne plus me contacter » → `complete`, `Perdu`, aucune relance.

## 4. Réactivation par thème
Pour chaque nouvelle session ou offre apparue dans le calendrier, proposer dans le récapitulatif les fiches `En veille` ou `Dormant` de la même offre.

Sortie : client / signal / relance n° / action / prochaine date / lien de la fiche.
