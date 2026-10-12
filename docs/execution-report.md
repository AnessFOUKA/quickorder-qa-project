## 5. Analyse de campagne

### Vue d'ensemble
- Nombre total de tests considérés : 24 cas de test ciblés sur les parcours critiques + 10 anomalies identifiées lors des vérifications exploratoires.
- Pass : 0
- Fail : 24
- Blocked : 0
- Skipped : 0

### Résultat synthétique
Le produit ne répond pas aux critères de mise en production sur les parcours financiers, de commande et d'API. Les anomalies détectées concernent directement le cœur du business : validation du panier, calcul du total, disponibilité du restaurant, gestion du stock, sécurité des données et intégrité des commandes.

### Principaux risques
- Risque financier : coupons, livraison, total et paiement peuvent être calculés ou validés de manière incorrecte.
- Risque opérationnel : des commandes peuvent être créées pour des restaurants fermés, avec des produits indisponibles ou en quantité dépassant le stock.
- Risque de sécurité : les appels API peuvent exposer les données d'un autre client ou contourner l'authentification pour lire les commandes d'Alice par défaut.
- Risque de fraude / intégrité : un paiement PayPal ou cash virtuellement non valide peut dupliquer ou accepter une commande incohérente.
- Risque de conformité métier : les statuts de commande peuvent revenir en arrière, ce qui casse la traçabilité et la logique de traitement.

### Recommandation Go / No-Go
- Recommandation : No-Go
- Justification : le produit présente plusieurs défauts critiques et majeurs sur les parcours fondamentaux. Le niveau de risque est incompatible avec une mise en production sans correction préalable et validation de régression.
- Décision : corriger en priorité les anomalies critiques de paiement, de stock, d'API, de restaurant fermé et de coupon, puis soumettre à une nouvelle campagne de tests fonctionnels et sécurité avant toute mise en production.