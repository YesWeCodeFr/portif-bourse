---
tracker_id: ""
type: initiative
title: "Portif Bourse"
sources:
  - _bmad-output/brainstorm-portif-bourse/brainstorm-portif-bourse.md
companions: []
after: []
risk: high
---

# Portif Bourse

## Description

Portif Bourse permet à un investisseur de suivre son portefeuille boursier en enregistrant ses opérations et en consultant sa composition et son évolution, sans lui recommander d’acheter ou de vendre.

## Outcome

Un investisseur peut constater l’état et la performance latente de son portefeuille à partir de ses opérations enregistrées et de cours récupérés automatiquement.

## Requirements

- **R1** (brainstorm, « Noyau retenu ») : l’utilisateur peut enregistrer un achat avec l’action, la quantité, le prix payé et la date ; l’état mis à jour de la position est affiché après l’opération, avec la quantité détenue et le prix moyen recalculé.
- **R2** (brainstorm, « Noyau retenu ») : pour chaque action détenue, l’application récupère automatiquement un cours et son historique, puis permet de visualiser son évolution depuis le premier achat jusqu’à aujourd’hui.
- **R3** (brainstorm, « Informations et vues » et « Comportement des opérations et des cours ») : chaque ligne du portefeuille présente le nom de l’action, la quantité, le cours retenu, sa valeur, le coût d’acquisition et la plus-value ou moins-value latente ; en l’absence de cours du jour, le dernier cours connu est utilisé et sa date est signalée au-delà de deux jours ouvrés de marché.
- **R4** (brainstorm, « Noyau retenu » et « Informations et vues ») : l’état actuel du portefeuille montre sa composition par action et sa valeur totale, incluant les liquidités disponibles.
- **R6** (décision de session) : au premier démarrage, le portefeuille est vide et les liquidités valent zéro ; l’utilisateur choisit une devise par défaut et peut saisir facultativement un montant initial.
- **R7** (décision de session) : lors de la saisie d’un achat, l’utilisateur sélectionne une place boursière parmi Euronext Paris, Xetra, Borsa Italiana, London Stock Exchange, NYSE et Nasdaq ; la liste peut être étendue ultérieurement.
- **R8** (décision de session) : l’utilisateur peut saisir un ticker et obtenir via une API une liste de correspondances avec leurs informations de marché, puis sélectionner l’instrument voulu pour son achat.
- **R9** (décision de session) : la V1 prend en charge les devises EUR et USD ; elle conserve un historique des taux EUR/USD et convertit une transaction en devise étrangère vers la devise par défaut au taux de la date de transaction.
- **R10** (décision de session) : pour chaque instrument détenu, l’application récupère le dernier cours connu, calcule sa valeur, la valeur totale du portefeuille et le pourcentage de cette position dans le total.
- **R11** (décision de session) : pour chaque instrument détenu, l’application calcule et affiche la plus-value ou moins-value en montant et en pourcentage par rapport au coût d’acquisition.
- **R12** (décision de session) : les historiques de cours des instruments et des taux de change récupérés par l’application sont conservés dans ses données et restent consultables automatiquement lors des utilisations ultérieures.
- **R13** (décision de session) : chaque transaction enregistrée est conservée avec ses informations de saisie et l’utilisateur peut consulter la liste des transactions passées.
- **R14** (décision de session) : après l’enregistrement d’un achat, le montant total de la transaction est déduit des liquidités disponibles et le nouveau solde est reflété dans l’état du portefeuille.
- **R15** (décision de session) : chaque transaction conserve son montant et sa devise d’origine, tandis que les calculs du portefeuille et le débit des liquidités sont réalisés dans la devise par défaut avec le taux historique de la date de transaction.
- **R16** (décision de session) : lors de la restitution de la composition actuelle, chaque position et la valeur totale sont converties et affichées dans la devise par défaut avec le dernier taux de change connu.

## Constraints

- **C1** (brainstorm, « Vision ») : l’application est un outil de consultation et de suivi ; elle ne formule aucune recommandation d’achat ou de vente.
- **C2** (décision de session) : la V1 est destinée à un seul utilisateur ; elle ne gère ni comptes multiples, ni partage, ni collaboration.
- **C3** (décision de session) : les frais de transaction, dividendes et opérations sur titres sont exclus de la V1.

## Done when

- Un achat enregistré met à jour une position et son prix moyen de façon vérifiable.
- Des cours et historiques sont affichés pour les actions détenues selon le périmètre de marché retenu.
- L’écran de portefeuille présente les valeurs de ligne, la composition par action et la valeur totale incluant les liquidités.
- Un cours de remplacement trop ancien est identifiable avec sa date de dernière mise à jour.
- La répartition sectorielle est explicitement hors périmètre V1 et pourra être ajoutée ultérieurement.

## Boundaries

- L’application ne fournit ni conseil, ni signal, ni recommandation d’investissement.
- Les ventes, frais de transaction, dividendes et opérations sur titres sont exclus de la V1 ; les ventes restent prévues comme évolution future.
- Le classement et la répartition par secteur sont reportés après la V1.

## References

- brainstorming — _bmad-output/brainstorm-portif-bourse/brainstorm-portif-bourse.md, Vision à Points à préciser

## Notes

- Decision: 2026-11-04 — La saisie initiale de liquidités est facultative ; l’utilisateur choisit la devise par défaut.
- Decision: 2026-11-04 — La V1 prend en charge EUR et USD et convertit les transactions étrangères à la date de transaction selon un historique de taux.
- Decision: 2026-11-04 — Les six places Euronext Paris, Xetra, Borsa Italiana, London Stock Exchange, NYSE et Nasdaq sont incluses dans la V1 ; d’autres places pourront être ajoutées plus tard.
- Decision: 2026-11-04 — L’évolution de chaque instrument et les pourcentages de portefeuille et de performance sont prioritaires ; le classement sectoriel est reporté après la V1.
- Direction: La V1 vise un seul utilisateur sans comptes ni partage.
- Direction: Alpha Vantage et Twelve Data sont des pistes pour la recherche de symboles ; couverture, quotas et droits d’utilisation restent à vérifier avant choix.
- Open question: Choisir l’API de recherche de symboles et vérifier sa couverture des six places, des actions, ETF et ETC, ses limites de quota et ses droits d’utilisation.
- Open question: Choisir la source des taux de change, la règle lorsqu’aucun taux n’existe à la date de transaction et le mode de présentation des montants d’origine et convertis.
- Open question: Définir la politique de rafraîchissement des historiques conservés, la détection des données manquantes et la correction d’une cotation révisée par la source.
- Direction: Chaque apport ou retrait de liquidité porte une date ; le dépôt initial reçoit par défaut le jour de saisie et peut être daté historiquement.
- Open question: Confirmer si les apports et retraits de liquidité sont des opérations V1, et si la date du dépôt initial est modifiable ou seulement fixée au jour de saisie.
- Decision: 2026-11-04 — Les transactions sont normalisées dans la devise par défaut pour les calculs ; le montant et la devise d’origine restent conservés.
- Decision: 2026-11-04 — La valorisation courante des positions en devise étrangère utilise le dernier taux de change connu vers la devise par défaut.
- Decision: 2026-11-04 — Les ventes sont hors périmètre de la V1 et seront réexaminées dans une version future.
- Decision: 2026-11-04 — Aucun epic de socle technique séparé n'est créé à ce stade ; le premier epic porte la mise en place minimale nécessaire à un parcours vertical fonctionnel.
