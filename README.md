# Update Guard : flux de vulnérabilités

Ce dépôt publie un flux **compact et signé** des vulnérabilités corrigeables de WordPress (extensions, thèmes, noyau), consommé par le mu-plugin [Update Guard](https://github.com/Hermitsde/wp-update-guard). Il est régénéré **chaque jour** (vers 5 h UTC) par une tâche GitHub Actions du dépôt Update Guard ; un commit n'est créé que si le contenu change.

Les données proviennent de **Wordfence Intelligence** (Defiant, Inc.), réutilisées selon leur licence : voir `LICENSE-wordfence.md`. Le flux ne contient ni titres ni descriptions, seulement ce qu'il faut pour décider d'une mise à jour.

## Fichiers

- `feed.jsonl` : le flux (UTF-8, une ligne JSON par logiciel).
- `feed.jsonl.sig` : signature ed25519 détachée (base64) des octets exacts de `feed.jsonl`.

## Format v1

Ligne 1, l'en-tête : `format`, `generated_at` (horodatage Unix), `source`, `attribution`, `license_url`, `entries` (nombre de lignes suivantes).

Lignes suivantes, triées par `t` puis `s` : `{"t":"plugin|theme|core","s":"slug","v":[…]}`. Chaque faille de `v` : `i` (identifiant Wordfence), `c` (CVE ou vide), `x` (score CVSS ou 0), `f` (versions corrigées), `a` (plages affectées `[de, inclus, jusqu'à, inclus]`, `*` = sans borne). Seules les failles non informatives, déjà corrigées, sont publiées.

## Vérification

Vérifier la signature avec la clé publique d'Update Guard (`sodium_crypto_sign_verify_detached`, signature décodée de base64) :

    php tools/verify-feed.php DOSSIER CLE_PUBLIQUE_B64

Un flux dont la signature ne correspond pas doit être ignoré.

## Attribution

Wordfence Intelligence est fourni par Defiant, Inc. Copyright 2012-2026 Defiant Inc. Conditions : https://www.wordfence.com/wordfence-intelligence-terms-and-conditions/
