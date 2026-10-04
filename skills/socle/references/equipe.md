# Équipe et routage du récepteur principal

## Annuaire (à remplir)
| Personne | Rôle | Adresses | ID base de suivi |
|---|---|---|---|
| <Collaborateur 1> | Commercial / Account manager | <adresses> | <id> |
| <Collaborateur 2> | Dirigeant / expert | <adresses> | <id> |
| <Collaborateur 3> | Administration et finance | <adresses> | <id> |
| Adresse générique | Accueil | contact@<domaine> | — |

## Étape 0 — Identifier la boîte connectée
`search_threads` avec `in:sent newer_than:30d` (pageSize 5), lire l'expéditeur, le rapprocher de l'annuaire. Mémoriser `BOITE`. Adresse inconnue : s'arrêter et demander.

## Étape 1 — Récepteur principal d'un message entrant
Sur le dernier message entrant du client, dans l'ordre :
1. **Transfert interne** : destinataire du transfert ; client = expéditeur d'origine.
2. **Champ À** : une seule adresse interne → c'est elle ; plusieurs → celle nommée dans la formule d'appel, sinon la première.
3. **Copie seulement** : la personne en copie, marquée `Récepteur : en copie` ; si le destinataire est un tiers, créer la fiche sans répondre.
4. **Fil existant** : l'auteur du dernier message interne du fil.
5. **Adresse générique** : pas de récepteur personnel ; responsable selon le type de demande, `Action humaine : transférer`.

## Étape 2 — Le récepteur dicte la boîte de réponse
- Récepteur = `BOITE` → brouillon dans cette boîte, signé par cette personne.
- Récepteur ≠ `BOITE` → pas de brouillon ; fiche assignée au récepteur ; `Brouillon : à rédiger par l'agent de <personne>` ou `Action humaine : transférer le mail`.

## Étape 3 — Responsable métier
| Type de demande | Responsable |
|---|---|
| Demande commerciale standard | <Account manager> |
| Conseil, expertise, partenariats | <Dirigeant> |
| Appels d'offres, institutionnel | <Dirigeant> |
| Facture, paiement | <Finance> |

Si le responsable diffère du récepteur : accusé de réception court, fiche assignée aux deux.
