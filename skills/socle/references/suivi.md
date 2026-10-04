# Base de suivi : fiche normalisée et adaptateurs

La base de suivi est la mémoire partagée de l'agent. Quel que soit le connecteur (`SUIVI` dans `config.md`), la **fiche** est la même ; seule change la façon de la stocker (`connecteurs.md`, section 3).

## Statuts
La base n'a besoin que de trois états. Le statut détaillé est porté par la fiche.

| Statut de la base | Statuts détaillés couverts | Sens |
|---|---|---|
| ouvert (`to do`) | Nouvelle demande, Qualification, Offre à chiffrer | Action humaine attendue |
| en cours (`in progress`) | Offre envoyée, Négociation, En veille, Dormant | Balle chez le client, relance programmée |
| clos (`complete`) | Gagné, Perdu | Dossier clos |

## Champs de la fiche
- **Nom** : `<Entreprise> — <objet court>`. Jamais de téléphone, d'email ni de montant.
- **Assignés** : récepteur principal + responsable métier.
- **Échéance = date de la prochaine relance** : c'est le champ que `suivi.lister_dues` interroge.
- **Priorité** : urgent = demande urgente ou relance du client · élevée = chaud · normale = tiède · basse = froid / dormant.

## Bloc fiche
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
- Fil email : <lien>
- Brouillon : prêt (<date>) | à rédiger par l'agent de <personne> | non
- Offre financière à chiffrer : oui | non
- Action humaine : <action ou « aucune »>
- À compléter : <manques ou « rien »>
- Contexte CRM : <étape, affaire liée, désinscrit oui/non> | aucun
- Dossier client : <lien> | aucun
- Dossier mis à jour le : AAAA-MM-JJ | jamais
- Synthèse : <une phrase tirée du dossier> | aucune
- Tâche liée : <lien ou « aucune »>
```
Sous le bloc : `## Résumé` (3 à 5 lignes : besoin, effectif, dates souhaitées, contraintes).

## Adaptateurs
Quand la base a de vrais champs, les renseigner **en plus** du bloc (le bloc reste la référence).

### ClickUp
Une liste de suivi. Statuts `to do` / `in progress` / `complete`. Bloc fiche dans la description, échéance native = prochaine relance, priorité native, commentaires = journal. Les champs personnalisés éventuels, dont le nom correspond à une ligne de la fiche, sont renseignés aussi.

### Notion
Une base de données. Propriétés recommandées : `Nom` (titre), `Statut` (sélection : Ouvert / En cours / Clos), `Statut détaillé` (sélection), `Prochaine relance` (date), `Priorité` (sélection), `Responsable` (personne), `Client` (texte), `Offre` (texte), `Fil email` (URL), `Nb relances` (nombre). Bloc fiche dans le corps de la page, commentaires = journal.

### Airtable
Une table. Champs recommandés : mêmes noms que ci-dessus (`Nom`, `Statut`, `Statut détaillé`, `Prochaine relance` en date, `Priorité`, `Responsable`, `Client`, `Offre`, `Fil email`, `Nb relances`, `Fiche` en texte long, `Journal` en texte long).

### Drive (Google Sheet)
Un onglet `Suivi`, une ligne par fiche, colonnes = mêmes noms, plus `Fiche` (texte du bloc) et `Journal` (ajouts datés). Relire la feuille juste avant chaque écriture.

## Journal
Chaque action de l'agent ajoute une entrée courte :
`[Agent <BOITE> · AAAA-MM-JJ] <action> — <raison>`

## Dédoublonnage (avant toute création)
1. `suivi.chercher` dans la base, **fiches closes incluses**, par lien du fil et nom d'entreprise.
2. Chercher aussi les autres bases ou listes accessibles, et le CRM si actif (affaire ouverte) → `Tâche liée`.
3. Fiche close pour le même besoin : ne pas recréer, la signaler. Un nouveau besoin du même client = nouvelle fiche.

## Date modifiée à la main
Si l'échéance diffère de `Prochaine relance`, un humain l'a changée : recopier l'échéance dans la fiche, `Date fixée manuellement : oui`, ne plus la recalculer tant qu'elle n'est pas passée.
