# Revue de fraîcheur technologique — architecture Portif Bourse

Date de revue : 2026-11-04
Document vérifié : `architecture-portif-bourse.md` révisé, incluant AD-10 à AD-13 et l'enveloppe Test & Operations.

## Verdict

Les choix technologiques engagés restent actuels et compatibles à la date de revue : Node.js 24 LTS, TypeScript 7.x, Vue 3.x, Fastify 5.x et Nginx pour le frontal HTTPS/reverse proxy. Les nouvelles décisions de conception (contrats d'adaptateurs, données canoniques, résolution externe avant mutation atomique, tests de contrat) ne contiennent pas de claim de version obsolète.

La principale réserve restante est la reproductibilité : les plages de versions doivent être résolues en versions exactes au scaffold et verrouillées par lockfile/images. Le moteur de base serveur, Docker/Compose et les fournisseurs de données restent explicitement différés, donc non engagés par cette revue.

## Vérifications officielles

### Node.js 24 LTS — confirmé

La page officielle des releases liste v24 comme LTS au 4 novembre 2026. La documentation officielle de migration indique un support jusqu'à fin avril 2028. Le choix est compatible avec Fastify 5, qui exige Node.js 20 ou supérieur.

Sources :

- https://nodejs.org/en/about/previous-releases
- https://nodejs.org/en/blog/migrations/v22-to-v24
- https://fastify.dev/docs/latest/Guides/Migration-Guide-V5/

Action : fixer le patch courant de la ligne 24, l'image Docker Node correspondante et la politique de mise à jour au scaffold.

### TypeScript 7.x — confirmé, à verrouiller

Le site officiel TypeScript indique que TypeScript 7.0 est disponible. La documentation recommande une installation par projet et un lockfile afin de garantir la cohérence entre machines.

Source : https://www.typescriptlang.org/download/

Action : fixer la version exacte et vérifier la compatibilité avec le plugin Vue, Vite et les outils Fastify.

### Vue 3.x — confirmé

La documentation officielle identifie Vue 3 comme la version majeure actuelle et recommande Vue 3 pour un nouveau projet. Pour une application complète, elle recommande Composition API et Single-File Components. Vue 2 est hors support depuis le 31 décembre 2023.

Sources :

- https://vuejs.org/about/faq
- https://vuejs.org/guide/introduction

### Vite — décision encore à prendre, mais option officielle recommandée

Le guide officiel Vue indique que le scaffold standard utilise Vite et que Vite est adapté lorsque le SSR n'est pas nécessaire. Le document révisé laisse encore le framework de build frontend dans `Deferred`, ce qui est cohérent ; le scaffold devra toutefois acter explicitement Vue 3 + Vite, avec sa version.

Source : https://vuejs.org/guide/quick-start

### Fastify 5.x — confirmé

Le guide officiel Fastify 5 confirme le support de Node.js 20+. La documentation LTS précise que les versions majeures sont testées avec les lignes Node.js supportées par la politique LTS. Fastify 5 est donc compatible avec Node.js 24 LTS.

Sources :

- https://fastify.dev/docs/latest/Guides/Migration-Guide-V5/
- https://fastify.dev/docs/v5.10.x/Reference/LTS/

Point utile pour AD-7/AD-12 : Fastify 5 exige des schémas JSON complets pour les paramètres, querystrings, corps et réponses ; les contrats REST versionnés devront les fournir.

### Nginx — confirmé pour AD-8

La documentation officielle Nginx confirme ses rôles de serveur HTTP et reverse proxy. La documentation HTTPS confirme la terminaison TLS via `listen 443 ssl`, certificat, clé privée et TLS 1.2/1.3. Nginx convient donc au frontal prévu.

Sources :

- https://nginx.org/en/
- https://nginx.org/en/docs/http/configuring_https_servers.html
- https://nginx.org/en/docs/http/ngx_http_ssl_module.html

La version/image Nginx, les en-têtes proxy, le renouvellement des certificats et la configuration de sécurité restent à fixer avant le déploiement public, comme indiqué dans `Deferred`.

## Vérification des décisions révisées

- AD-10 ne prétend pas qu'une valeur de portefeuille peut être produite sans données de marché : il sépare correctement coût d'acquisition et valorisation courante, et exige la date/fraîcheur des données.
- AD-11 est cohérent avec SQLite, mémoire et un futur moteur serveur ; l'exigence de tests de contrat réduit le risque de divergence, mais le moteur serveur et la matrice exacte de migrations restent à décider.
- AD-12 définit un modèle canonique raisonnable pour adapter plusieurs fournisseurs ; la couverture, la licence, les quotas et la disponibilité restent correctement dans le spike différé.
- AD-13 évite un appel réseau dans une transaction d'écriture et conserve le taux/source/date avant mutation ; il faudra encore définir les timeouts, la politique d'échec et l'idempotence avant la story d'achat.
- L'enveloppe de tests et opérations est compatible avec les choix de stack ; elle ne fixe pas indûment une solution d'orchestration.

## Findings restants

1. Remplacer les plages `24 LTS`, `7.x`, `3.x`, `5.x` par des versions exactes au scaffold ; conserver lockfiles et images versionnées.
2. Décider explicitement Vite pour le build frontend et documenter le mode de service des fichiers compilés dans le conteneur applicatif.
3. Choisir le moteur serveur transactionnel et vérifier migrations, décimales, contraintes et tests de contrat sur ce moteur.
4. Avant toute exposition publique, spécifier TLS/certificats, secrets, limites de payload, rate limiting, logs et restauration ; AD-9 est un risque accepté, pas une configuration production-ready.
5. Pour AD-13 et les stories d'écriture, préciser timeout/retry, absence de taux, échec fournisseur et clé d'idempotence.

## Conclusion

La version révisée de l'architecture est techniquement à jour au 2026-11-04. Aucun choix de version engagé n'est stale ou incompatible. Les points ci-dessus sont des décisions de scaffold, d'exploitation ou de spike à traiter avant implémentation/déploiement, et non des corrections urgentes de l'architecture spine.
