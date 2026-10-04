---
name: tri-demandes
description: Trie les emails entrants, identifie le récepteur principal, classe la demande et crée ou met à jour la fiche CRM. Utiliser quand l'utilisateur dit « trie mes mails », « quelles demandes clients sont arrivées », « qui doit traiter ce mail », ou comme première étape de tour-quotidien.
---

# Tri des demandes entrantes

Lire `../socle/references/regles.md`, `equipe.md` et `crm.md`.

1. **Boîte connectée** : étape 0 de `equipe.md` → `BOITE`.
2. **Fils à trier** : `in:inbox newer_than:3d -in:sent -category:promotions -category:social` en excluant les expéditeurs automatiques. Toujours `get_thread` (les aperçus cachent les derniers messages).
3. **Bruit** (notifications, newsletters, candidatures, sollicitations de prestataires, échanges internes) : ne pas créer de fiche, compter dans le récapitulatif.
4. **Transfert déjà fait** : si un message interne du fil transfère déjà la demande à un collègue, ne pas relancer le transfert ; le récepteur est ce collègue.
5. **Récepteur principal** : étape 1 de `equipe.md`.
6. **Classement** : type de demande, offre (intitulé canonique), intention (chaud = demande explicite avec échéance), urgence (relance du client ou échéance sous 5 jours → `urgent`).
7. **Dédoublonnage** puis écriture CRM selon `crm.md` (tâches closes et autres listes incluses). Une tâche close pour le même besoin n'est pas recréée.
8. **Suite** : récepteur = `BOITE` → `reponse-client` ; sinon fiche seulement et `Action humaine` ; réponse à une relance ou offre → `relances.md`.

Sortie : tableau client / type / récepteur / responsable / action / lien de la fiche.
