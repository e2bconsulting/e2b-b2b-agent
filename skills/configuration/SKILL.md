---
name: configuration
description: Configure l'agent de relation client B2B — choix des connecteurs (email Gmail ou Outlook, CRM HubSpot ou Brevo, base de suivi ClickUp, Notion, Airtable ou Drive), préparation de la base de suivi et vérification de la connexion. Utiliser quand l'utilisateur dit « configure l'agent », « quels connecteurs utiliser », « prépare la base de suivi », « passe sur Outlook / Notion / Airtable », ou au premier lancement.
---

# Configuration de l'agent

Lire `../socle/references/config.md`, `connecteurs.md` et `suivi.md`.

## Déroulé
1. **Inventaire** : lister les connecteurs réellement connectés à la session (email, CRM, bases, agenda, fichiers).
2. **Choix** : poser une seule question à choix multiples couvrant EMAIL, CRM (ou aucun), SUIVI, AGENDA. Ne proposer que des connecteurs connectés ; pour un connecteur absent, indiquer comment le connecter plutôt que de contourner.
3. **Base de suivi** : vérifier si la base existe déjà.
   - Elle existe → vérifier les propriétés attendues (`suivi.md`, section Adaptateurs) et lister les manques.
   - Elle n'existe pas → proposer de la créer avec les champs recommandés (ClickUp : liste à trois statuts ; Notion : base avec les propriétés ; Airtable : table avec les champs ; Drive : feuille `Suivi`). Créer uniquement après accord explicite.
4. **Équipe** : remplir avec l'utilisateur l'annuaire de `equipe.md` (personnes, adresses, rôles, identifiants).
5. **Test à blanc** : lecture d'un fil récent (EMAIL), lecture de la base (SUIVI), recherche d'un contact connu (CRM). Aucun brouillon, aucune écriture.
6. **Sortie** : le bloc `config.md` rempli, la liste des manques (catalogue, ton, signatures, kit d'offre), et la commande de planification quotidienne.
