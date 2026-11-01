# Messagerie souveraine de l'administration

Prototype pour le sprint pré-hackathon 2026 (Hackathon 226). Messagerie instantanée chiffrée de bout en bout, hébergeable sur l'infrastructure de l'État.

## Concept et architecture
Serveur Matrix Synapse + client Element Web + PostgreSQL + Keycloak, orchestrés par Docker Compose. Le chiffrement (Olm/Megolm) est assuré par Element dans le navigateur : le serveur ne stocke que du texte chiffré. Aucune cryptographie n'a été développée par l'équipe.

## Niveau d'engagement open source
Étendre et adapter (Synapse, Element Web).

## Technologies et licences (à vérifier sur les dépôts officiels)
Synapse, Element Web, PostgreSQL, Keycloak, Docker. Notre code : AGPL-3.0 (voir LICENSE).

## Installation
1. Copier .env.example en .env et choisir les mots de passe.
2. Générer la configuration de Synapse avec la commande generate de l'image Synapse (voir docs/).
3. Adapter synapse/homeserver.yaml : PostgreSQL, enable_registration false, federation_domain_whitelist [], trusted_key_servers [], encryption_enabled_by_default_for_room_type all.
4. docker compose up -d
5. Créer les comptes de démonstration (voir docs/comptes-demo.md).

## Preuves
- Le serveur ne lit rien : une recherche du message dans la base renvoie 0 résultat, seuls des événements m.room.encrypted sont stockés.
- Révocation : voir docs/revocation.md.

## Limites connues
- Keycloak (TOTP) est déployé mais pas encore relié à Synapse : Synapse exige HTTPS pour un fournisseur OIDC (reverse proxy à ajouter).
- Pas de HTTPS en démonstration locale ; Keycloak en mode développement.
- Un agent révoqué conserve les messages déjà reçus sur son appareil.
- Le déploiement n'est pas encore entièrement automatisé : la configuration de Synapse et la création des comptes sont manuelles.
- Les métadonnées (qui, quand, quel salon, taille, adresse IP) restent visibles du serveur.
