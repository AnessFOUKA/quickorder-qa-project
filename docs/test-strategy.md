# Stratégie de test

## Périmètre
- vérification de la disponibilité de l'application et de l'API
- inscription, connexion et déconnexion d'un client
- consultation des restaurants, des détails d'un restaurant et de ses produits
- recherche et consultation des produits
- ajout et retrait de produits du panier, modification des quantités et calcul du total
- validation des coupons et vérification des règles de commande
- passage d'une commande et consultation de son détail et de son historique
- annulation d'une commande lorsque son statut le permet
- gestion du profil et des adresses du client
- gestion des favoris et dépôt d'un avis
- authentification de l'API avec une clé valide, absente ou invalide
- consultation des restaurants et produits, validation d'un coupon et gestion des commandes via l'API
- vérification des données retournées, des messages d'erreur et des codes HTTP de l'API

## Hors périmètre
- tests de charge, de performance à grande échelle et de montée en charge
- tests sur plusieurs navigateurs et appareils, sauf vérification rapide sur un navigateur courant
- paiement réel et vérification auprès de prestataires de paiement externes
- envoi réel d'e-mails, de SMS ou de notifications externes
- audit de sécurité approfondi et test d'intrusion
- modification du code source pendant la campagne initiale
- tests des fonctionnalités d'administration, sauf si elles sont explicitement incluses dans les scénarios de la campagne

## Parcours critiques
- un nouveau client crée un compte, se connecte et consulte les restaurants
- un client choisit un restaurant, consulte ses produits et ajoute des articles au panier
- un client modifie les quantités et vérifie que le sous-total, les frais et le total sont cohérents
- un client applique un coupon valide et vérifie le montant de la réduction
- un client passe une commande avec des données valides et retrouve la commande dans son historique
- un client tente de commander avec un produit indisponible, une quantité invalide ou un montant insuffisant et reçoit un refus explicite
- un client consulte une commande et tente de l'annuler selon son statut
- un appel API avec une clé valide récupère les données ou réalise une action autorisée
- un appel API sans clé ou avec une clé incorrecte est refusé sans divulguer de données

## Risques
- total, frais ou réduction incorrects : le client peut payer un montant erroné
- commande créée avec un produit indisponible, une quantité invalide ou un restaurant fermé : la commande ne peut pas être honorée
- commande non enregistrée ou absente de l'historique : le client ne peut pas suivre son achat
- contrôle d'accès incorrect : un client peut consulter ou modifier les données d'un autre client
- clé API non contrôlée : un utilisateur non autorisé peut accéder aux données ou actions de l'API
- validation incorrecte des coupons : réduction appliquée à tort ou coupon valide refusé
- erreurs peu compréhensibles dans le panier ou le paiement : le client ne sait pas comment terminer son parcours

## Types de tests
- tests fonctionnels nominaux pour vérifier les parcours valides de bout en bout
- tests de limites pour les quantités, les montants, le stock et les champs obligatoires
- tests négatifs pour les identifiants inconnus, les données mal formées et les accès refusés
- tests des règles métier pour les coupons, la disponibilité, les frais et les statuts de commande
- tests d'interface web pour vérifier les formulaires, les messages, la navigation et l'affichage des totaux
- tests API avec Swagger ou Postman pour contrôler les requêtes, les réponses JSON et les codes HTTP
- tests de non-régression ciblés après correction d'une anomalie
- tests exploratoires ciblés sur le panier, la commande et les cas d'erreur

## Priorisation risque × impact
- échelle de probabilité : 1 = faible, 2 = moyenne, 3 = élevée
- échelle d'impact : 1 = faible gêne, 2 = parcours dégradé, 3 = perte financière, commande impossible ou exposition de données
- score de priorité = probabilité × impact ; 1 à 2 = basse, 3 à 4 = moyenne, 6 à 9 = haute
- priorité haute : montant total ou coupon incorrect, commande impossible ou incohérente, accès aux données d'un autre client, clé API contournable
- priorité moyenne : recherche ou affichage d'un produit incorrect, gestion du profil ou des favoris défaillante, message d'erreur imprécis
- priorité basse : défaut visuel mineur sans effet sur la compréhension ni sur la réalisation du parcours

