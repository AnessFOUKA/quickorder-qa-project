# Plan d'amélioration - QuickOrder

## Objectif
Corriger les défauts critiques identifiés lors de la campagne de test afin de sécuriser les parcours de commande, la fiabilité des prix et la confiance utilisateur avant toute mise en production.

## Priorisation des améliorations

### 1. Sécuriser l'API et isoler les données utilisateur
- Priorité : Critique
- Justification : l'API utilise un utilisateur par défaut (`alice@example.com`) pour les clients non connectés, ce qui expose des données d'un autre compte et violate les principes de sécurité et de confidentialité.
- Action : supprimer le fallback implicite, exiger une authentification de l'utilisateur réel et filtrer systématiquement les ressources sur l'ID du client connecté ou sur son rôle d'admin.
- Effet attendu : suppression du risque de fuite de données et conformité avec les exigences de sécurité applicative.

### 2. Contrôler strictement les règles métier avant la création de commande
- Priorité : Critique
- Justification : la commande peut être validée pour un restaurant fermé, avec un produit indisponible, un montant minimum non atteint, ou un stock insuffisant.
- Action : validator le restaurant (`is_open`), le panier (`stock` et `available`), le minimum de commande et la quantité demandée avant confirmation paie.
- Effet attendu : réduction importante des commandes impossibles ou incohérentes, ainsi que des erreurs de fulfillment.

### 3. Corriger la logique de paiement et les montants
- Priorité : Critique
- Justification : le paiement PayPal duplique les commandes et le paiement cash accepte des montants insuffisants, ce qui crée un risque financier et opérationnel direct.
- Action : empêcher la création de commandes multiples pour le même paiement, refuser les montants cash insuffisants, calculer la monnaie de manière explicite et bloquer les transactions invalides côté backend.
- Effet attendu : intégrité du paiement et meilleure fiabilité du processus de commande.

### 4. Vérifier le stock et les quantités avant et après validation
- Priorité : Haute
- Justification : les produits sans stock ou avec quantité dépassant le stock restent commandables et le système réduit le stock sans vérifier les limites.
- Action : ajouter une validation de stock au niveau de l'ajout au panier, du checkout et de l'API, et refuser les quantités excédant les disponibilités restantes.
- Effet attendu : évite les ruptures de stock, les commandes impossibles à exécuter et les stocks négatifs.

### 5. Rendre les promotions cohérentes avec l'offre commerciale
- Priorité : Haute
- Justification : le coupon ATLAS15 est annoncé à 15 % mais stocké à 10 %, ce qui crée une incohérence visible et une méfiance des clients.
- Action : centraliser les règles de coupon dans une source unique, valider le libellé marketing avec la configuration effective et afficher les montants calculés avant validation.
- Effet attendu : confiance client, réduction des abandons et exactitude du calcul de remise.

### 6. Mettre en place une validation de workflow du statut des commandes
- Priorité : Haute
- Justification : le statut d'une commande peut revenir en arrière, par exemple d'"en cours" ou "livré" à "confirmé" ou "annulé".
- Action : définir une machine à états métier avec transitions autorisées, gérer les validations par rôle et bloquer toute évolution incohérente.
- Effet attendu : traçabilité plus fiable et conformité aux règles de gestion des commandes.

## Plan de mise en œuvre recommandée
1. Sécuriser l'API et isoler les données utilisateur
2. Contrôler strictement les règles métier avant la création de commande
3. Corriger la logique de paiement et les montants
4. Vérifier le stock et les quantités avant et après validation
5. Rendre les promotions cohérentes avec l'offre commerciale
6. Mettre en place une validation de workflow du statut des commandes