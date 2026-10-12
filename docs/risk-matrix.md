# Matrice des risques — QuickOrder

## Objectif et périmètre

Cette matrice priorise les risques qualité observés pendant l'audit de QuickOrder, avant toute mise en production. Elle couvre les parcours de commande et de paiement, les stocks, les coupons, l'accès aux données via l'API et le workflow des commandes. Les risques ci-dessous sont étayés par les anomalies documentées dans les bug reports ; ils ne remplacent pas un audit de sécurité ni des tests de charge, qui sont hors périmètre de la campagne actuelle.

## Échelle d'évaluation

- **Probabilité (P)** : 1 = faible / scénario rare ; 2 = moyenne / dépend de conditions précises ; 3 = élevée / scénario courant ou défaut reproductible dans les conditions décrites.
- **Impact (I)** : 1 = gêne mineure ; 2 = parcours dégradé ou incohérence métier ; 3 = perte financière, commande impossible ou incohérente, ou exposition de données personnelles.
- **Score** : `P × I`.
- **Niveau** : 1–2 = bas ; 3–4 = moyen ; 6–9 = haut. Un score de 9 est traité comme **critique** pour l'ordre de correction, compte tenu de l'impact sécurité ou opérationnel.

Les probabilités sont des estimations qualitatives fondées sur les scénarios reproductibles rapportés ; elles ne représentent pas une fréquence statistique de production.

## Registre des risques

| ID | Risque et conséquence | P | I | Score / niveau | Réponse prioritaire | Éléments associés |
|---|---|---:|---:|---|---|---|
| R-01 | **Exposition des commandes d'un autre client par l'API** : en l'absence d'une identité client authentifiée, l'API utilise Alice par défaut. Un appelant muni de la clé API peut donc obtenir ses commandes sans être authentifié comme Alice. | 3 | 3 | **9 — Haut (critique)** | Supprimer l'identité de repli ; authentifier le client réel et appliquer les contrôles d'accès par propriétaire/ rôle sur chaque endpoint. Ajouter des tests négatifs multi-comptes pour `GET /api/orders` et `GET /api/orders/<id>`. | [BR-07](bug-reports/BR-07.md), [Plan d'amélioration](improvement-plan.md) |
| R-02 | **Commandes PayPal dupliquées** : une confirmation de paiement peut créer deux commandes pour le même panier, entraînant une double préparation et un risque de traitement ou débit en double. | 2 | 3 | **6 — Haut** | Rendre la création idempotente et ne créer qu'une commande par paiement ; vérifier le nombre de commandes et les lignes enregistrées par un test de régression. | [BR-02](bug-reports/BR-02.md), [Plan d'amélioration](improvement-plan.md) |
| R-03 | **Paiement cash insuffisant accepté** : une commande peut être validée même si la somme reçue est inférieure au total ; la monnaie calculée peut être négative. | 2 | 3 | **6 — Haut** | Refuser côté serveur tout montant inférieur au total ; vérifier la validation, le total et la monnaie sur les limites autour du montant dû. | [BR-04](bug-reports/BR-04.md), [Plan d'amélioration](improvement-plan.md) |
| R-04 | **Commande acceptée alors que les conditions de vente ne sont pas satisfaites** : un restaurant fermé reste commandable ou le minimum de commande n'est pas appliqué. La commande risque de ne pas être honorée ou de contourner une règle commerciale. | 2 | 3 | **6 — Haut** | Revalider côté serveur l'ouverture du restaurant et le minimum de commande au checkout et via l'API ; afficher un refus explicite et tester les cas juste en dessous/au seuil. | [BR-03](bug-reports/BR-03.md), [BR-05](bug-reports/BR-05.md), [Plan d'amélioration](improvement-plan.md) |
| R-05 | **Vente au-delà du stock disponible** : une commande peut dépasser le stock et le décrémenter sous zéro, causant une rupture et des commandes impossibles à exécuter. | 3 | 3 | **9 — Haut (critique)** | Vérifier disponibilité et quantité demandée sur le serveur à l'ajout au panier, au checkout et via l'API ; garantir que la mise à jour du stock ne peut pas le rendre négatif. Tester stock nul, stock exact et dépassement. | [BR-06](bug-reports/BR-06.md), [Plan d'amélioration](improvement-plan.md) |
| R-06 | **Remise affichée différente de la remise appliquée** : ATLAS15 annonce 15 % mais n'applique que 10 %, ce qui entraîne un montant inattendu et une perte de confiance. | 2 | 3 | **6 — Haut** | Aligner le texte promotionnel et la configuration du coupon dans une source unique ; vérifier le calcul et le montant présenté avant confirmation. | [BR-01](bug-reports/BR-01.md), [Plan d'amélioration](improvement-plan.md) |
| R-07 | **Transitions de statut incohérentes** : un administrateur peut faire revenir une commande livrée à un état antérieur ou l'annuler, ce qui compromet le suivi et la traçabilité. | 2 | 2 | **4 — Moyen** | Définir les transitions autorisées selon l'état courant et tester les transitions valides et interdites par rôle. | [BR-08](bug-reports/BR-08.md), [Plan d'amélioration](improvement-plan.md) |

## Ordre de traitement recommandé

1. **Avant tout Go** : traiter R-01 et R-05, compte tenu de l'exposition de données et du risque de stock négatif.
2. Corriger R-02, R-03 et R-04 pour fiabiliser la création des commandes, les montants encaissés et les règles de vente.
3. Corriger R-06 et R-07, puis exécuter les tests de régression correspondants.
4. Relancer une campagne ciblée sur les parcours impactés et confirmer les corrections avant de réévaluer le Go/No-Go.

## Décision de mise en production

**Recommandation actuelle : No-Go.** Les risques hauts touchent la confidentialité des données client, l'intégrité du stock, les paiements et la validation des commandes. La mise en production ne devrait être réévaluée qu'après correction des risques hauts et réussite des tests de régression et de sécurité ciblés.

## Références

- [Stratégie de test](test-strategy.md) — périmètre, parcours critiques et échelle de priorisation.
- [Rapport d'exécution](execution-report.md) — résultats de campagne et recommandation Go/No-Go.
- [Plan d'amélioration](improvement-plan.md) — actions correctives proposées.
