---
id: 1
type: story
title: "Initialiser un portefeuille"
parent: epic-portefeuille-transactions
covers: [R2]
after: []
hitl: true
risk: high
refined: true
---

# Initialiser un portefeuille

## Description

Une personne lance l'application et vérifie le parcours initial ; l'utilisateur crée l'état initial d'un portefeuille vide, choisit sa devise par défaut et voit zéro liquidité.

## Requirements

1. **Créer l'état initial vide** (R2)
   **Given** aucune configuration de portefeuille n'existe
   **When** l'application est lancée pour la première fois
   **Then** un portefeuille est créé avec zéro position et zéro liquidité

2. **Choisir la devise par défaut** (R2)
   **Given** l'écran d'initialisation est affiché
   **When** l'utilisateur choisit EUR ou USD et valide
   **Then** la devise choisie devient la devise par défaut du portefeuille
   **And** cette devise est visible dans l'état du portefeuille

3. **Laisser l'apport initial vide** (R2)
   **Given** une devise par défaut est choisie
   **When** l'utilisateur ne saisit aucun montant initial et termine l'initialisation
   **Then** l'initialisation est acceptée
   **And** les liquidités restent à zéro

4. **Refuser une initialisation sans devise** (R2)
   **Given** l'écran d'initialisation est affiché
   **When** l'utilisateur tente de continuer sans choisir EUR ou USD
   **Then** l'application demande de choisir une devise
   **And** aucun portefeuille utilisable n'est créé avec une devise indéterminée

5. **Retrouver l'état initial** (R2)
   **Given** l'initialisation a été validée
   **When** l'application est relancée
   **Then** le portefeuille conserve la devise par défaut choisie
   **And** il affiche toujours zéro position et zéro liquidité

## Boundaries

- La saisie d'un montant initial et sa date sont traitées par la story 1.2.
- Aucun instrument, cours, achat ou conversion de devise n'est traité par cette story.
- La V1 reste mono-utilisateur, sans compte ni partage.

## References

- parent — _bmad-output/initiative-portif-bourse/tickets/epic-portefeuille-transactions/epic-portefeuille-transactions.md

## Notes

- Assumption: Le choix de devise initial est limité à EUR et USD, conformément au périmètre V1.
- High-risk check: Une personne vérifie manuellement le parcours de premier lancement et la relance de l'application avant de considérer la story terminée.
