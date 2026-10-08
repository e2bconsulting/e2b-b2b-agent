# Base de suivi : fiche normalisée et adaptateurs

La base de suivi est la mémoire partagée de l'agent. Quel que soit le connecteur (`SUIVI` dans `config.md`), la **fiche** est la même ; seule change la façon de la stocker (`connecteurs.md`, section 3).

## Statuts
La base n'a besoin que de trois états. Le statut détaillé est porté par la fiche.

| Statut de la base | Statuts détaillés couverts | Sens |
|---|---|---|
| ouvert (`to do`) | Nouvelle demande, Qualification, Offre à chiffrer | Action humaine attendue |
| en cours (`in progress`) | Offre envoyée, Négociation, En veille, Dormant, Gagné (après-vente en cours) | Balle chez le client ou après-vente en cours, relance programmée |
| clos (`complete`) | Perdu ; Gagné une fois l'après-vente terminé | Dossier clos : jamais relancé ni recréé (voir « Garde d'état et dédoublonnage ») |

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

## Garde d'état et dédoublonnage (obligatoire avant toute création de fiche, tout brouillon et toute relance)

**Principe : un client + un besoin = une seule fiche, ouverte ou close. L'agent ne crée jamais une fiche sans avoir d'abord cherché la fiche existante, closes comprises, et lu son statut.**

### 0. Définition de « close » et « ouverte »
Une fiche, ou toute tâche d'une autre base ou liste, est **close** dès qu'elle est clôturée dans la base où elle se trouve, **quel que soit le nom de son statut et quel que soit son niveau** (tâche ou sous-tâche, n'importe quelle liste, dossier ou espace). C'est la **nature** du statut qui décide, jamais son nom (`complete`, `closed`, `done`, `terminé`, `Clos`, statut personnalisé…).

| Base | La fiche est close quand… |
|---|---|
| ClickUp | le type de son statut est `Done` ou `Closed` ; à défaut `date_closed` (ou `date_done`) est renseigné ; en dernier recours seulement, si ni type de statut ni date de clôture ne sont lisibles, son statut s'appelle `complete`, `closed`, `done`, `terminé` ou `clôturé` |
| Notion | `Statut` = Clos (ou statut du groupe « Terminé / Complete » si la base utilise une propriété Statut native) |
| Airtable | `Statut` = Clos |
| Drive (Sheet) | colonne `Statut` = Clos |

Une fiche **ouverte** est toute fiche non close, quel que soit son statut (`to do`, `in progress` ou statut personnalisé). Si le statut n'est pas lisible et qu'aucun critère ne s'applique, la traiter comme ouverte et le signaler dans le récapitulatif.

### 1. Rechercher (toujours, fiches closes comprises)
1. `suivi.chercher`, **fiches closes incluses**, toutes pages. Comparer la ligne `Fil email` des fiches candidates (même entreprise ou même domaine email) avec l'identifiant du fil traité, puis le nom de l'entreprise et le domaine email. Une recherche ne lit pas toujours le corps des fiches : lire les fiches candidates.
2. Chercher aussi les autres bases ou listes accessibles, **tâches closes comprises**, et le CRM si actif (affaire ouverte) → `Tâche liée` (lecture seule, jamais modifiée). Une tâche close dans une autre base pour le même client et le même besoin suit la même règle qu'une fiche close (§2 et §3) : l'agent ne la rouvre jamais ; si les trois conditions du §3 sont réunies, il crée une nouvelle fiche dans sa propre base avec `Tâche liée` vers elle.
3. Consigner la recherche : entrée de journal sur la fiche trouvée, ou mention dans le récapitulatif si aucune fiche n'existe.

### 2. Lire le statut de la fiche trouvée (`suivi.statut`)
| Statut | Conduite |
|---|---|
| Ouverte (`to do`, `in progress` ou tout statut non clos) | Mettre à jour cette fiche. Ne jamais en créer une seconde pour le même client et le même besoin. |
| Close (voir §0) | Aucune création de fiche, aucun brouillon, aucune relance, aucune modification de l'échéance ni du statut détaillé. Signaler « fiche close ignorée » dans le récapitulatif. Seule exception : la réouverture (§3). |
| Aucune fiche | Créer la fiche normalement. |

### 3. Réouverture d'une fiche close
Une fiche close n'est rouverte, ou remplacée par une nouvelle fiche, que si les **trois conditions** sont réunies :
1. Le dernier message du fil est écrit **par le client** (expéditeur externe). Un message de l'équipe (brouillon envoyé, relance, transfert interne, mise en copie), une notification automatique ou une réponse automatique ne rouvre jamais une fiche.
2. Ce message est **postérieur à la date de clôture** (ClickUp : `date_closed` ; autres bases : date du dernier changement de statut ou de l'entrée de journal de clôture).
3. Il contient une **demande nouvelle** : nouvelle question, nouvelle session ou nouvelles dates, nouveaux participants, demande de devis ou de programme, nouvelle offre. Un simple remerciement, un accusé de réception ou un « bien reçu » n'en est pas une.

Conduite lorsque les trois conditions sont réunies :
- **Même besoin** (même fil, ou même offre ou service) → rouvrir **la même fiche** : statut `to do`, statut détaillé et signal recalculés (`relances.md`), `Nb relances` remis à 0, `Date fixée manuellement : non`, `Dernier contact client` mis à jour, entrée de journal `[Agent <BOITE> · AAAA-MM-JJ] Fiche rouverte — nouveau message client du <date> : <motif en une ligne>`.
- **Besoin distinct** (autre offre, autre périmètre) → nouvelle fiche, avec `Tâche liée` vers la fiche close.
- **Doute** sur l'un des trois critères → ne rien créer ni rouvrir ; le signaler dans le tableau des actions humaines.

Fiche `Perdu` après un refus définitif ou pour un contact désinscrit du CRM : si le client réécrit, la réouverture sert uniquement à répondre à son message, jamais à relancer : aucune échéance de relance n'est programmée (`Prochaine relance` vide, `Nb relances` figé).

### 4. Ne pas retraiter un fil déjà traité
Si le dernier message du client dans le fil n'est pas plus récent que `Dernier contact client` de la fiche (ouverte ou close), ne rien écrire : ni entrée de journal, ni mise à jour, ni brouillon. Ce contrôle empêche la fenêtre de lecture glissante (3 derniers jours) de recréer ou de relancer le même dossier chaque jour.

### 5. Seule exception d'écriture sur une fiche close
`dossier-client` peut écrire sur une fiche close, et uniquement les trois lignes `Dossier client`, `Dossier mis à jour le` et `Synthèse`. Aucun autre skill ne modifie une fiche close, hors réouverture justifiée au §3.

## Date modifiée à la main
Si l'échéance diffère de `Prochaine relance`, un humain l'a changée : recopier l'échéance dans la fiche, `Date fixée manuellement : oui`, ne plus la recalculer tant qu'elle n'est pas passée.
