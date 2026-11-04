---
name: "Adversarial architecture review — Portif Bourse"
type: architecture-review
reviewer: "adversarial lower-level consistency"
target: ../architecture-portif-bourse.md
created: "2026-11-04"
updated: "2026-11-04"
---

# Verdict

La révision a correctement fermé les principaux trous du premier passage : séparation ledger/valorisation, contrat canonique des données externes, contrat commun des adaptateurs, résolution FX hors transaction d'écriture, décimales JSON et enveloppe d'exploitation. Le spine est maintenant exploitable comme direction d'implémentation, mais reste volontairement non déterministe sur quelques décisions explicitement différées. Ces décisions doivent être fermées avant les stories concernées.

## Findings encore ouverts

### F-1 — Solde insuffisant, concurrence et unité de validation restent différés

AD-4 garantit l'atomicité achat + effet cash, mais ne définit pas encore si un achat est rejeté lorsque la liquidité dérivée est insuffisante, ni comment sérialiser deux mutations concurrentes. La section Deferred reconnaît le point, mais AD-11 exige déjà des tests d'atomicité et d'idempotence avant que la règle soit fixée.

**Risque :** deux adaptateurs peuvent produire des résultats différents, ou deux requêtes peuvent toutes deux accepter un achat sur le même solde.

**À fermer avant la story d'achat :** politique de solde négatif ; isolation/verrouillage de la mutation ; ordre de calcul et d'écriture ; test de concurrence. Retirer ou préciser l'exigence d'idempotence d'AD-11 tant que la clé d'idempotence n'est pas définie.

### F-2 — Le ledger canonique n'est pas encore assez précis

AD-3 parle d'« effets » et AD-11 de reconstruction, mais aucun contrat ne définit la forme d'une ligne de registre : signe, devise de l'effet cash, coût d'acquisition, quantité, instrument et relation entre l'événement métier et ses effets. Une implémentation peut encore calculer la liquidité en devise d'origine et une autre en devise par défaut, tout en prétendant respecter AD-3/AD-6.

**Risque :** soldes, prix moyens et poids différents entre mémoire, SQLite et moteur serveur.

**À fermer avant les stories d'écriture :** modèle canonique des transactions/effets, matrice par type V1 (`APPORT_LIQUIDITE`, `ACHAT`), devise de calcul, signes, champs immuables et arrondi à chaque étape.

### F-3 — La date effective du FX et les jours sans cotation restent ambigus

AD-6 impose de conserver un taux et AD-12 impose une paire orientée/date effective, mais ne fixe pas l'algorithme entre date de transaction et date de cotation. Le contrat reste compatible avec « même jour uniquement », « dernière cotation antérieure » ou « taux suivant ».

**Risque :** une transaction historique USD peut être normalisée différemment selon le fournisseur ou l'adaptateur.

**À fermer dans le spike marché/FX :** convention EUR/USD, sens du taux, calendrier, règle des jours non cotés, précision/arrondi du taux, et comportement d'erreur lorsqu'il n'existe pas de taux acceptable. Stocker explicitement la date effective choisie, pas seulement la date de conversion.

### F-4 — « Chaîne JSON canonique » manque de règles lexicales

La convention fixe désormais les décimales en chaînes et les schémas versionnés, ce qui résout la perte de précision. Elle ne fixe pas le format lexical : zéros de tête/fin, signe, exposant, échelle par champ et normalisation de `-0`.

**Risque :** comparaison, validation et arrondi divergent entre frontend, backend et adaptateurs.

**À fermer au scaffold :** grammaire décimale interdite en notation exponentielle, normalisation de `-0`, échelle/précision par champ et politique d'arrondi partagée.

### F-5 — Idempotence HTTP et identité d'événement ne sont pas encore modélisées

Le spine mentionne des identifiants opaques et reporte la clé d'idempotence. Une reprise après timeout peut donc créer deux apports ou deux achats. AD-11 annonce des tests d'idempotence alors qu'aucune clé, contrainte ou fenêtre de déduplication n'est définie.

**Risque :** double débit de liquidité ou double position.

**À fermer avant toute mutation REST :** clé d'idempotence obligatoire ou déduplication par identifiant d'événement ; portée, durée de conservation et réponse d'une répétition ; contrainte persistante correspondante.

### F-6 — Moteur serveur encore ouvert malgré une exigence transactionnelle

Le port et les migrations sont maintenant obligatoires, mais PostgreSQL et NoSQL restent candidats alors que leurs garanties transactionnelles, contraintes et stratégies d'indexation ne sont pas interchangeables. La compatibilité du contrat de persistance ne suffit pas à garantir la même isolation et le même coût de reconstruction.

**Risque :** SQLite passe les tests mais le futur adaptateur serveur ne peut pas honorer la même mutation atomique ou les mêmes performances.

**À fermer avant l'adaptateur serveur :** choix du moteur ou critères éliminatoires explicites ; sémantique d'isolation attendue ; contraintes et index du registre ; test de restauration et de reconstruction sur le moteur choisi.

### F-7 — Ordre déterministe des événements encore non spécifié

AD-11 exige un ordre déterministe, AD-5 prévoit uniquement une date métier et réserve l'intrajournalier au futur. L'horodatage technique UTC existe, mais le spine ne dit pas s'il constitue le tie-breaker, ni quoi faire en cas d'égalité d'horodatage.

**Risque :** historique affiché, validation de liquidité ou résultat de reconstruction différents après import/rejeu, même si la V1 ne supporte pas encore les ventes.

**À fermer :** ordre canonique par identifiant séquentiel/position d'insertion, ou `createdAt + id` ; comportement lors d'une restauration et d'un import.

### F-8 — Frontend dans le conteneur unique : service des assets et SPA fallback non définis

AD-8 fixe le conteneur, mais pas le processus qui sert `frontend/`, le routage des URLs SPA, le proxy `/api`, le health check et l'arrêt gracieux. Nginx peut servir les assets, ou Fastify peut le faire ; les deux respectent le spine mais exigent des images et configurations différentes.

**Risque :** rechargement d'une route frontend en 404, image non reproductible ou health check trompeur.

**À fermer au scaffold/déploiement :** stratégie unique de service des assets, fallback SPA, `/health`/readiness, multi-stage build et gestion de migration au démarrage.

### F-9 — Risque d'exposition publique correctement reconnu, mais garde-fous encore absents

AD-9 et l'enveloppe opérationnelle reconnaissent que la V1 publique sans authentification n'est pas production-ready. Toutefois, limitation de débit, taille de payload, logs, secrets et restauration sont encore différés ; le diagramme continue de présenter Internet comme entrée directe.

**Risque :** lecture/modification publique, abus fournisseur, remplissage de base et perte de données.

**À fermer avant le premier déploiement public :** décision explicite d'environnement de test seulement, ou contrôle réseau/IP/VPN ; rate limiting et limites payload ; sauvegarde restaurable ; renouvellement certificats ; logs sans secrets. Ce risque ne doit pas être traité comme une simple story fonctionnelle.

## Points résolus par la révision

- Fichier de données canonique (instrument, quote, FX), provenance et horodatage : couvert par AD-12.
- Séparation coût/valorisation courante et signal de fraîcheur : couverte par AD-10 et la convention de données externes.
- Taux externe hors transaction d'écriture : couvert par AD-13.
- Contrats mémoire/SQLite/serveur et reconstruction après redémarrage : couverts par AD-11 et Test & Operations Envelope.
- Décimales REST en chaînes et schémas versionnés : couverts par la convention JSON.

## Priorité

Avant les stories d'écriture : F-1, F-2, F-3, F-4 et F-5. Avant l'adaptateur serveur : F-6 et F-7. Avant le premier déploiement : F-8 et F-9.

La revue ne trouve pas de contradiction fatale dans les AD révisées ; elle recommande de conserver le statut `draft` jusqu'à ce que les décisions différées ci-dessus soient transformées en contrats testables ou en décisions explicitement acceptées.
