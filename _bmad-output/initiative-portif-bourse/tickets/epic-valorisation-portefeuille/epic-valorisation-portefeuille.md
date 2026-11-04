---
tracker_id: ""
id: 3
type: epic
title: "Valorisation et consultation du portefeuille"
parent: "initiative-portif-bourse"
covers: ["R3", "R4", "R10", "R11", "R16"]
sources: [_bmad-output/initiative-portif-bourse/initiative-portif-bourse.md]
companions: [_bmad-output/initiative-portif-bourse/architecture-portif-bourse/architecture-portif-bourse.md]
after: [epic-instruments-cours-devises]
assignee: ""
refined: false
hitl: false
risk: medium
---

# Valorisation et consultation du portefeuille

## Description

L'application restitue une vue actuelle du portefeuille comme une projection de lecture recalculée à partir des transactions, des cours et des taux persistés.

## Outcome

Un utilisateur peut comprendre la valeur et la performance latente de chaque position et de l'ensemble du portefeuille dans sa devise par défaut.

## Done when

- Chaque ligne affiche quantité, cours retenu, valeur, coût d'acquisition et performance.
- La valeur totale inclut les liquidités dérivées et les pourcentages de répartition sont calculés.
- Les positions étrangères et le total sont convertis avec le dernier taux connu.
- Un cours de remplacement trop ancien est signalé avec sa date.
- L'évolution depuis le premier achat est consultable et l'intégration est vérifiée dans l'environnement de déploiement de la V1.

## Boundaries

- Les données de marché et de change sont consommées depuis l'Epic 2.
- Le classement sectoriel est explicitement reporté après la V1.
- Aucune recommandation d'investissement n'est produite.

## References

- parent — _bmad-output/initiative-portif-bourse/initiative-portif-bourse.md, Requirements R3, R4, R10, R11, R16
- architecture — _bmad-output/initiative-portif-bourse/architecture-portif-bourse/architecture-portif-bourse.md

## Notes

- Decision: 2026-11-04 — L'epic dépend de l'ensemble des données persistées par l'Epic 2 avant de produire la vue valorisée.
- Decision: 2026-11-04 — La valorisation courante est une projection reconstruite depuis les positions et liquidités dérivées, les derniers cours et les taux de change ; elle expose la fraîcheur ou l'absence de données.
