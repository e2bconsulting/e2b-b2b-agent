---
name: tour-quotidien
description: Lance un passage complet de l'agent de relation client B2B pour la personne connectée — tri des nouveaux emails, récepteur principal, fiches CRM, brouillons et relances dues — puis rend un récapitulatif. Utiliser quand l'utilisateur dit « lance l'agent », « passage du matin », « traite mes demandes clients », ou depuis une tâche planifiée.
---

# Passage complet

N'écrit que des brouillons email et des fiches dans la base de suivi configurée.

## Déroulé
1. Lire `../socle/references/regles.md`, `config.md`, `connecteurs.md` et `equipe.md`. Exécuter les contrôles de démarrage de `config.md` ; si les connecteurs ne sont pas choisis, appeler le skill `configuration`.
2. Identifier `BOITE` (étape 0 de `equipe.md`). Annoncer : « Passage de l'agent pour <personne> (<adresse>) ».
3. `tri-demandes` sur la fenêtre depuis le dernier passage (`newer_than:1d` par défaut ; `newer_than:3d` le lundi ou après un passage manqué).
4. Pour chaque fiche dont le récepteur principal est `BOITE` et sans brouillon : `reponse-client` ou `apres-vente`.
5. `moteur-relances`.
6. Le lundi (ou sur demande), pour le responsable du pipeline : `revue-hebdo`.
7. Contrôle final : aucun montant dans un brouillon ; chaque brouillon a sa fiche à jour ; chaque fiche créée a une échéance.

## Récapitulatif (toujours)
- En-tête : mails lus, écartés, fiches créées, fiches mises à jour, brouillons prêts, relances préparées.
- Tableau des brouillons : client, objet, type, lien du brouillon, lien de la fiche.
- Tableau des actions humaines : transferts, offres à chiffrer, pièces à joindre, informations `À compléter`.
- Si rien à faire : le dire en une ligne, sans créer de fiche.

## Planification
Une tâche planifiée par personne, chacune dans sa propre session connectée à sa propre boîte : jours ouvrés, 9h00 (fuseau local), consigne « Lance tour-quotidien » (+ « et revue-hebdo » le lundi pour le responsable du pipeline).
