---
tracker_id: ""
id: 2
type: epic
title: "Instruments, cours et devises"
parent: "initiative-portif-bourse"
covers: ["R2", "R7", "R8", "R9", "R12"]
sources: [_bmad-output/initiative-portif-bourse/initiative-portif-bourse.md]
companions: []
after: [epic-portefeuille-transactions]
assignee: ""
refined: false
hitl: false
risk: high
---

# Instruments, cours et devises

## Description

L'application permet de retrouver un instrument par ticker sur les places retenues, puis de récupérer et persister les cours historiques ainsi que les taux EUR/USD nécessaires au suivi du portefeuille.

## Outcome

Un instrument et un taux de change peuvent être identifiés, récupérés, historisés et réutilisés lors d'une session ultérieure.

## Done when

- Une recherche par ticker retourne des correspondances exploitables avec leur place de marché.
- Les six places V1 et les instruments actions, ETF et ETC sont pris en charge selon la couverture de la source choisie.
- Les historiques de cours et de taux EUR/USD sont enregistrés et réutilisables.
- Une transaction étrangère peut être convertie avec le taux historique de sa date.
- L'intégration est vérifiée dans l'environnement de déploiement de la V1.

## Boundaries

- Les transactions et leur modèle sont fournis par l'Epic 1.
- La présentation de la composition et des performances est fournie par l'Epic 3.
- Le choix précis des fournisseurs, leurs quotas et leurs licences est traité dans cet epic.

## References

- parent — _bmad-output/initiative-portif-bourse/initiative-portif-bourse.md, Requirements R2, R7, R8, R9, R12

## Notes

- Decision: 2026-11-04 — Le premier travail à risque est un spike de couverture et de persistance pour la recherche de ticker, les cours et EUR/USD.
- Open question: Choisir les fournisseurs, les règles de taux manquant, la politique de rafraîchissement et la correction des cotations révisées.
