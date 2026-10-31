# Portif Bourse — synthèse du brainstorming

## Vision

Portif Bourse permet de suivre un portefeuille boursier en enregistrant les opérations et en visualisant sa composition actuelle ainsi que son évolution. L’application sert à consulter et suivre le portefeuille ; elle ne recommande pas d’acheter ou de vendre.

## Noyau retenu pour une première version

- Enregistrer un achat avec l’action, la quantité, le prix payé et la date.
- Récupérer automatiquement les cours et leur historique pour les actions détenues.
- Afficher l’évolution du cours de chaque action depuis la date du premier achat jusqu’à aujourd’hui.
- Présenter l’état actuel du portefeuille. Le graphique principal représente sa composition actuelle.

## Informations et vues envisagées

- Pour chaque ligne : nom de l’action, quantité détenue, cours actuel, valeur totale, coût d’acquisition et plus-value ou moins-value latente.
- Afficher les liquidités disponibles. La valeur totale du portefeuille comprend la valeur des actions et les liquidités.
- Montrer la répartition du portefeuille par action, par exemple avec un graphique, et permettre de classer les actions par secteur pour afficher la répartition sectorielle.
- Consulter l’historique des achats et des ventes.
- En complément du graphique principal, explorer l’évolution quotidienne sur un an du portefeuille global, des actions détenues et des secteurs. Une vue peut séparer la valeur des actions et les liquidités.

## Comportement des opérations et des cours

- Après un achat, la quantité détenue augmente et le prix moyen d’achat est recalculé à partir des actions déjà détenues et du nouvel achat.
- Après une vente, la quantité détenue diminue et les liquidités augmentent. Le prix moyen d’achat des actions restantes ne change pas.
- Après une opération, afficher le nouvel état du portefeuille.
- Si le cours du jour n’est pas disponible, afficher le dernier cours connu. Ne pas signaler son ancienneté s’il date du jour même ; au-delà de deux jours ouvrés de marché, afficher un symbole et la date de dernière mise à jour.

## Points à préciser

- L’enregistrement des ventes fait-il partie de la première version ?
- Comment le portefeuille et les liquidités sont-ils initialisés ?
- Quelles actions et quelles places boursières sont couvertes, et quelle source fournit les cours ?
- Les frais de transaction, dividendes, opérations sur titres et devises entrent-ils dans le périmètre ?
- Quels graphiques complémentaires sont prioritaires après la composition actuelle ?

---

Cette synthèse reprend les idées et décisions exprimées pendant la séance de brainstorming. Le journal complet se trouve dans [`.memlog.md`](./.memlog.md).
