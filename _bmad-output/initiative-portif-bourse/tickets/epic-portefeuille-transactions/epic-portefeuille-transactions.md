---
tracker_id: ""
id: 1
type: epic
title: "Gestion du portefeuille et des transactions"
parent: "initiative-portif-bourse"
covers: ["R1", "R6", "R13", "R14", "R15"]
sources: [_bmad-output/initiative-portif-bourse/initiative-portif-bourse.md]
companions: []
after: []
assignee: ""
refined: false
hitl: false
risk: high
---

# Gestion du portefeuille et des transactions

## Description

Le portefeuille peut être initialisé et alimenté par des achats historisés. Les opérations conservent leur devise d'origine et mettent à jour les positions ainsi que les liquidités dans la devise par défaut.

## Outcome

Un utilisateur peut enregistrer un achat et retrouver un portefeuille cohérent, avec ses liquidités, ses positions et son historique de transactions.

## Done when

- Le portefeuille est initialisé vide avec une devise par défaut choisie par l'utilisateur.
- Un apport initial facultatif et un achat daté mettent à jour les liquidités et la position.
- Le prix moyen et les montants normalisés sont calculés de façon vérifiable.
- La transaction reste consultable avec ses informations et sa devise d'origine.
- Le parcours est intégré et vérifié dans l'environnement de déploiement de la V1.

## Boundaries

- Les cours externes et les taux de change sont fournis par l'Epic 2.
- La valorisation et les vues de portefeuille sont fournies par l'Epic 3.
- Les ventes, frais, dividendes et opérations sur titres restent hors périmètre.

## References

- parent — _bmad-output/initiative-portif-bourse/initiative-portif-bourse.md, Requirements R1, R6, R13, R14, R15

## Notes

- Decision: 2026-11-04 — Le tracer bullet de l'epic est l'initialisation en EUR suivie de l'enregistrement d'un achat daté, de la mise à jour de la position et du débit des liquidités.
- Open question: Confirmer si les apports et retraits de liquidité sont tous deux des opérations V1 ; le dépôt initial reste facultatif.
