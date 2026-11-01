# Intention produit — suivi de portefeuille boursier

## Intention produit

Offrir une photographie fiable et neutre d’un portefeuille boursier, afin de consulter son état courant, d’en suivre l’évolution dans le temps, de saisir les achats et ventes, et de calculer les positions ainsi que les plus-values ou moins-values. La valeur totale du portefeuille est l’information prioritaire ; elle agrège les titres détenus et le cash disponible.

## Périmètre

- Vue par position : quantité détenue, dernier cours coté, valeur de position et plus-value ou moins-value latente par rapport au prix d’achat.
- Saisie des achats et ventes, avec mise à jour cohérente des positions et du cash disponible.
- Historique des transactions : achats, ventes, apports et retraits de cash.
- Vue historique de la valeur du portefeuille, incluant les titres et le cash, avec une lecture de tendance globale.
- Consultation de l’évolution depuis la création du portefeuille, avec une granularité adaptée à la période (jour, semaine, mois ou année).
- Repère des plus-values ou moins-values latentes depuis le 1er janvier, pour aider l’utilisateur à estimer son imposition.

## Non-objectifs

- Recommander des investissements ou prendre des décisions financières.
- Analyser les causes d’une variation de marché ou indiquer une conduite à tenir.

## Capacités visibles clés

- Afficher au premier plan la valeur totale, puis son détail par titres et cash.
- Montrer le dernier cours de chaque valeur détenue, accompagné de sa date ou ancienneté de mise à jour.
- Rendre immédiatement lisibles l’état et la performance de chaque position.
- Après une transaction, refléter la création ou l’évolution de la ligne concernée et la variation correspondante du cash.
- Présenter l’évolution quotidienne du portefeuille sous forme de graphique afin de rendre la hausse ou la baisse perceptible rapidement.

## Attentes de données et de confiance

- Les cours affichés doivent être disponibles et suffisamment frais pour inspirer confiance.
- Le contexte temporel d’un cours doit être explicite et interprétable selon les jours de bourse, notamment les jours ouvrés.
- Les transactions, positions et cash doivent rester visiblement cohérents entre eux.
- Le produit restitue des faits de façon neutre ; l’interprétation et les décisions restent à l’utilisateur.
- La continuité de l’historique doit préserver l’évolution du portefeuille, les opérations et les repères annuels.

## Questions ouvertes

- Faut-il compléter le graphique global par l’historique des cours de chaque valeur détenue ?
- Un tableau des valeurs quotidiennes doit-il accompagner le graphique historique ?
- Quel historique minimal conserver, la préférence exprimée étant un suivi quotidien sur un an et une consultation depuis la création du portefeuille ?
