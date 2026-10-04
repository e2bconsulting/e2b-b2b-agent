---
name: socle
description: Base de connaissances et règles communes de l'agent de relation client B2B (règles, configuration et connecteurs, équipe et routage, base de suivi, catalogue, ton, kit d'offre, FAQ, relances). Charger avant toute réponse à un client ou quand l'utilisateur demande « quelles sont nos règles », « qui traite cette demande », « que proposons-nous ».
---

# Socle — règles et connaissances partagées

Source de vérité de tous les autres skills. Lire uniquement les fichiers utiles à la tâche.

| Fichier | Contenu | Quand le lire |
|---|---|---|
| `references/regles.md` | Règles non négociables | **Toujours** |
| `references/equipe.md` | Adresses, rôles, routage du récepteur principal | Tri et rédaction |
| `references/config.md` | Choix des connecteurs (email, CRM, base de suivi, agenda, fichiers) | **Au début de chaque passage** |
| `references/connecteurs.md` | Opérations abstraites et outil correspondant par connecteur | Toute lecture ou écriture externe |
| `references/suivi.md` | Statuts, fiche normalisée, journal, dédoublonnage, adaptateurs | Toute écriture de fiche |
| `references/catalogue.md` | Offres, intitulés canoniques, formats | Réponse commerciale |
| `references/ton-signatures.md` | Registre, modèles, signatures | Rédaction |
| `references/kit-offre.md` | Pièces jointes standard | Offres |
| `references/faq-objections.md` | Réponses validées | Rédaction |
| `references/relances.md` | Moteur de relances | Relances |

## Rappel des règles absolues
1. Créer uniquement des brouillons (Gmail ou Outlook), jamais d'envoi.
2. Aucun prix, remise ou montant : écrire `[OFFRE FINANCIÈRE À JOINDRE]` et marquer la fiche « à chiffrer ».
3. Le CRM est en lecture seule ; un contact désinscrit n'est jamais relancé.
4. Rédiger uniquement depuis la boîte du récepteur principal.
5. Ne citer une date que si elle a été lue dans le calendrier le jour même ; ne jamais citer un client à un autre.
6. Ne rien inventer : formulation prudente et `À compléter` dans la fiche.
