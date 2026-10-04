# Conventions CRM (exemple : ClickUp)

Le CRM est la mémoire partagée. Exemple avec une liste ClickUp unique ; transposable à tout outil qui offre statut, échéance, assigné, description et commentaires.

## Paramètres à renseigner
- Workspace : `<id>` · Liste de suivi : `<id>`
- Statuts de la liste : `to do` · `in progress` · `complete`

| Statut CRM | Statuts détaillés | Sens |
|---|---|---|
| `to do` | Nouvelle demande, Qualification, Offre à chiffrer | Action humaine attendue |
| `in progress` | Offre envoyée, Négociation, En veille, Dormant | Balle chez le client, relance programmée |
| `complete` | Gagné, Perdu | Dossier clos |

## Champs natifs
- Nom : `<Entreprise> — <objet court>`.
- Assignés : récepteur principal + responsable métier.
- **Échéance = date de la prochaine relance** (le moteur de relances l'interroge).
- Priorité : urgent = demande urgente · high = chaud · normal = tiède · low = froid.

## Fiche normalisée (début de description)
```
## Fiche agent
- Récepteur principal : <adresse> (<personne>)
- Boîte de réponse : <adresse> | en attente de transfert
- Client : <Entreprise> — <Prénom NOM>, <fonction> — <email>
- Type de demande : <types>
- Offre / service : <intitulé canonique>
- Statut détaillé : Nouvelle demande | Qualification | Offre à chiffrer | Offre envoyée | Négociation | En veille | Dormant | Gagné | Perdu
- Intention : chaud | tiède | froid | dormant
- Signal client : « <phrase exacte> »
- Motif de relance : sans réponse | rappel demandé | indécis | tiède | offre sans retour | refus temporaire | livrable | facture
- Prochaine relance : AAAA-MM-JJ
- Date fixée manuellement : non | oui
- Nb relances : <n>
- Dernier contact client : AAAA-MM-JJ
- Fil Gmail : <lien>
- Brouillon : prêt (<date>) | à rédiger par l'agent de <personne> | non
- Offre financière à chiffrer : oui | non
- Action humaine : <action ou « aucune »>
- À compléter : <manques ou « rien »>
- Tâche liée : <lien ou « aucune »>
```
Sous le bloc : `## Résumé` (3 à 5 lignes).

## Journal
Commentaire à chaque action : `[Agent <BOITE> · AAAA-MM-JJ] <action> — <raison>`.

## Dédoublonnage (avant toute création)
1. Chercher dans la liste de suivi, **tâches closes incluses**, par fil Gmail et nom d'entreprise.
2. Chercher dans les autres listes (par nom et domaine email) → renseigner `Tâche liée`.
3. Si une tâche close existe pour le même besoin, ne pas recréer : la signaler. Un nouveau besoin = nouvelle fiche.

## Date modifiée à la main
Si l'échéance diffère de `Prochaine relance`, un humain l'a changée : recopier l'échéance dans la fiche, `Date fixée manuellement : oui`, ne plus la recalculer tant qu'elle n'est pas passée.
