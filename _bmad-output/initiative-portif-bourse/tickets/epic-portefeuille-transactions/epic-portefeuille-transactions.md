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
refined: true
hitl: false
risk: high
---

# Gestion du portefeuille et des transactions

## Description

Le portefeuille peut être initialisé, alimenté par un apport de liquidité et alimenté par des achats historisés. Les opérations conservent leur devise d'origine et mettent à jour les positions ainsi que les liquidités dans la devise par défaut.

## Outcome

Un utilisateur peut enregistrer un achat et retrouver un portefeuille cohérent, avec ses liquidités, ses positions et son historique de transactions.

## Requirements

- **R1** (parent R1) : l'utilisateur peut enregistrer un achat avec l'instrument sélectionné, la quantité, le prix payé et la date ; la position mise à jour affiche la quantité détenue et le prix moyen recalculé.
- **R2** (parent R6) : au premier démarrage, le portefeuille est vide, les liquidités valent zéro et l'utilisateur choisit une devise par défaut ; il peut saisir ou non un apport initial, dont la date est facultative et, si elle est absente, prend la date de saisie.
- **R3** (parent R13) : chaque transaction enregistrée conserve les informations saisies et peut être retrouvée dans une liste des transactions passées.
- **R4** (parent R14) : après un achat, le montant total normalisé de la transaction est déduit des liquidités disponibles et le nouveau solde est reflété dans l'état du portefeuille.
- **R5** (parent R15) : chaque transaction conserve son montant et sa devise d'origine ; elle expose aussi le montant normalisé dans la devise par défaut à partir d'un taux fourni pour la date de transaction.

## Constraints

- **C1** (parent C1) : le portefeuille est un outil de suivi et ne formule aucune recommandation d'investissement.
- **C2** (parent C2) : la V1 ne gère qu'un seul utilisateur, sans comptes ni partage.
- **C3** (parent C3) : les ventes, frais, dividendes et opérations sur titres ne sont pas gérés dans cet epic.

## Done when

- Le portefeuille est initialisé vide avec une devise par défaut choisie par l'utilisateur.
- Un apport initial facultatif, avec date facultative, et un achat daté mettent à jour les liquidités et la position.
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
- Open question: Confirmer si les retraits de liquidité sont une opération V1 ; l'apport initial est inclus et sa date est facultative.
- Decision: 2026-11-04 — Les stories de cet epic portent le registre des transactions et la normalisation à partir d'un taux fourni ; la récupération des taux et des instruments appartient à l'Epic 2.
- Decision: 2026-11-04 — Une story dédiée couvre l'apport de liquidité initial ; sa date peut être absente et reçoit alors la date de saisie.
- Decision: 2026-11-04 — L'Epic 1 est planifié en six stories ordonnées : initialisation, apport initial, achat, normalisation, historique, puis consolidation.
