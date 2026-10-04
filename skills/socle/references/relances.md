# Moteur de relances contextuel

Le client fixe le rythme. Lire le **dernier message du client** dans le fil, en déduire un signal, puis calculer la prochaine relance. La date est stockée dans la date d'échéance de la tâche ClickUp et dans la ligne `Prochaine relance` de la fiche : elle tient même si elle tombe dans six mois.

## 1. Détection du signal (dans cet ordre)

| Ordre | Indices dans le message du client | Signal | Statut détaillé |
|---|---|---|---|
| 1 | « ne plus me contacter », « désinscrire », « pas intéressé », « non merci » | Refus définitif | Perdu |
| 2 | Date explicite : « le 15 janvier », « semaine du 12 », « dans 2 mois », « dans 3 semaines » | Rappel daté | En veille |
| 3 | Mois ou période : « en janvier », « à la rentrée » (= septembre), « après les fêtes », « après la clôture », « fin du trimestre » | Rappel daté | En veille |
| 4 | « pas de budget cette année », « l'année prochaine », « budget non validé » | Refus temporaire | Dormant |
| 5 | « plus tard », « pas pour le moment », « pas maintenant », « ultérieurement » | Tiède | En veille |
| 6 | « je dois en discuter », « je reviens vers vous », « validation interne », « je réfléchis » | Indécis | Négociation |
| 7 | Aucune réponse du client après une offre envoyée | Offre sans retour | Offre envoyée |
| 8 | Aucune réponse du client à une demande d'information interne (hors offre) | Sans réponse | Qualification |
| 9 | Oui, confirmation, bon de commande | Gagné | Gagné → passer la main à `apres-vente` |

Recopier la phrase exacte du client dans `Signal client`. Si le signal est ambigu, choisir « Indécis » et écrire `À compléter : signal ambigu, vérifier la date` dans la fiche.

## 2. Calcul de la date

Point de départ `D` = date du dernier message (client, ou interne pour les signaux 7 et 8).

| Signal | Relance n°1 | Suivantes si pas de retour | Angle de chaque relance |
|---|---|---|---|
| Rappel daté (date précise) | Date demandée moins 3 jours ouvrés | +10 j, puis +30 j → En veille à 90 j | « Comme convenu, je reviens vers vous… » + nouveauté (date de session, programme) ; puis relance courte ; puis point de situation |
| Rappel daté (mois / période) | Premier jour ouvré autorisé du mois ou de la période | +10 j, puis +30 j | Idem |
| Après une période creuse connue (fêtes, vacances) | 5 jours ouvrés après la fin de la période (vérifier la date de l'année) | +10 j, +30 j | Idem |
| Refus temporaire | Si `D` est entre janvier et août : 1er octobre de la même année. Si `D` est entre septembre et décembre : 2e semaine de janvier suivant | +21 j | Budget et plan annuel du client ; puis relance courte |
| Tiède | D + 45 j | D + 90 j, puis Dormant à D + 180 j | Nouveauté catalogue ou cas client ; nouvelle formule ; point annuel |
| Indécis | D + 30 j | D + 60 j, D + 90 j, D + 180 j | Résultat concret chez un client similaire ; nouvelle session ; contenu utile ou contenu utile ; besoins de l'année suivante |
| Offre sans retour | D + 4 j | D + 10 j, D + 21 j, puis En veille D + 90 j | Proposer un appel de 15 min ; nouvelle formule ou nouveaux créneaux ; message de clôture courtois « sauf avis contraire » ; réactivation avec nouveauté |
| Sans réponse (qualification) | D + 3 j | D + 10 j, puis Dormant | Reformuler la question en une ligne ; dernière relance |
| Refus définitif | Aucune | Aucune | Statut `complete`, `Nb relances` figé |

Plafond : au-delà de 6 relances sans aucune réponse depuis le premier contact, passer en Dormant avec une seule réactivation annuelle (2e semaine d'octobre).

## 3. Ajustement de la date (toujours appliqué en dernier)
Paramètres à adapter à votre pays et secteur :
- `JOURS_ENVOI` : par défaut mardi, mercredi, jeudi (meilleur taux de réponse B2B). Si la date tombe un autre jour, avancer au prochain jour autorisé.
- `PAUSE_ETE` : par défaut du 15 juillet au 31 août → reporter au premier mardi de septembre.
- `JOURS_FERIES` : liste des jours fériés fixes de votre pays, plus les fêtes variables (vérifier la date de l'année dans le calendrier ou par recherche) ; éviter ces jours et le lendemain.
- Ne jamais programmer deux relances le même jour pour le même client.

## 4. Exécution le jour J
Une relance n'est rédigée **que le jour où elle est due** (date d'échéance ≤ aujourd'hui) :
1. Relire le fil (`email.lire_fil`) et, si le CRM est actif, vérifier `crm.statut_desinscription` : un contact désinscrit sort du moteur (refus définitif). Si le client a écrit depuis la dernière action interne → annuler la relance, re-détecter le signal, recalculer.
2. Si un humain a déjà relancé (message interne plus récent que la fiche) → mettre à jour `Dernier contact`, recalculer à partir de ce message.
3. Sinon → brouillon de relance dans le fil (`replyToMessageId` = dernier message), angle de la ligne correspondante, jamais « je me permets de vous relancer ».
4. Incrémenter `Nb relances`, calculer la relance suivante, mettre à jour échéance + fiche, commenter le journal.

## 5. Dates fixées par un humain
Si `Date fixée manuellement : oui`, ne pas recalculer la date ; l'exécuter le jour venu. Après exécution, repasser à `non` et reprendre le calcul normal.

## 6. Réactivation par thème
Quand une nouvelle session apparaît dans le calendrier, chercher les fiches Dormant / En veille portant la même formation (ligne `Offre / service`) et proposer, dans le récapitulatif, de les relancer avec la nouvelle date (pas de brouillon automatique hors échéance, sauf si l'utilisateur le demande).
