# Procédure de révocation d'un agent

Objectif : un agent révoqué (départ, perte du téléphone) perd l'accès au serveur et aux nouveaux messages sur tous ses appareils.

## 1. Obtenir un jeton administrateur
Connexion du compte administrateur via POST /_matrix/client/v3/login. Le jeton est un secret : ne jamais l'enregistrer dans le dépôt.

## 2. Désactiver l'agent
POST /_synapse/admin/v1/deactivate/@AGENT:localhost avec le corps {"erase": false}, en-tête Authorization: Bearer <jeton>.

## 3. Vérification
- Base de données : la colonne deactivated de la table users vaut 1 pour l'agent.
- Client : l'agent est déconnecté et ne peut plus se reconnecter.
- Groupe : il ne reçoit plus les nouveaux messages.

## Limites connues
- Les messages déjà reçus restent lisibles sur l'appareil de l'agent révoqué.
- La désactivation est difficile à annuler.
- L'API /_synapse/admin ne doit pas être exposée à l'extérieur (à bloquer au niveau du reverse proxy).
