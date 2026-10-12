# Tests utilisateurs - QuickOrder

## Participants

### Utilisateur 1 — Marie, 29 ans, étudiante en master
- Profil : cliente régulière de services de livraison, utilise les apps de repas 2 à 3 fois par semaine.
- Contexte : passe des commandes le soir depuis son téléphone pendant ses études.
- Problème : a tenté de commander depuis un restaurant affiché comme "ouvert" et a constaté que certains produits ne sont plus disponibles sans message clair.
- Fréquence : souvent.
- Impact : frustrant ; la commande ne peut pas être finalisée facilement.
- Verbatim : "Je vois le restaurant et le produit, donc je pense que je peux commander. Mais au moment du panier, le système ne me dit pas clairement pourquoi le produit est bloqué."
- Hypothèse d'amélioration : afficher un message explicite de rupture de stock ou de fermeture du restaurant avant l'ajout au panier.
- Priorité : Haute

### Utilisateur 2 — Thomas, 34 ans, cadre, commande occasionnelle
- Profil : utilisateur soucieux des délais et de la clarté du parcours, commande environ une fois par semaine.
- Contexte : utilise l'application pendant sa pause déjeuner pour choisir un restaurant rapide.
- Problème : le système affichait un filtre de livraison "max 2,50 €" qui ne renvoyait pas les restaurants les moins chers, ce qui le poussait à sélectionner des options incohérentes.
- Fréquence : occasionnelle.
- Impact : perte de temps et doute sur la qualité de l'offre affichée.
- Verbatim : "J’ai choisi 2,50 € maximum, mais les restaurants les plus chers réapparaissaient. J’ai pensé que le filtre était cassé."
- Hypothèse d'amélioration : inverser la logique du filtre pour afficher uniquement les restaurants dont le frais de livraison est inférieur ou égal au seuil choisi.
- Priorité : Moyenne

### Utilisateur 3 — Lina, 27 ans, salariée, utilisatrice régulière de services de livraison
- Profil : consomme une commande de repas en moyenne 4 fois par semaine, très attentive aux promos et aux totaux.
- Contexte : passe des commandes depuis son ordinateur au retour du travail.
- Problème : a appliqué un coupon promo annoncé comme 15 % et le montant final n'a pas correspondu à l'offre affichée.
- Fréquence : régulière.
- Impact : méfiance vis-à-vis de la plateforme et risque d’abandon du panier.
- Verbatim : "Le code promo disait -15 %, mais le système a appliqué une réduction beaucoup plus faible. Je ne sais plus si je peux faire confiance au montant final."
- Hypothèse d'amélioration : afficher le pourcentage exact du coupon et valider le code promo avec le bon taux avant confirmation de commande.
- Priorité : Haute

### Utilisateur 4 — Mehdi, 41 ans, gestionnaire de projet, commande ponctuelle
- Profil : utilisateur qui compare les options et vérifie les montants avant de payer, commande environ deux fois par mois.
- Contexte : passe commande depuis un navigateur en fin de journée.
- Problème : a tenté de payer en cash avec un montant insuffisant et le système a accepté la commande sans message de blocage.
- Fréquence : occasionnelle.
- Impact : risque de commande invalide et de confusion sur le règlement.
- Verbatim : "J’ai saisi un montant en espèces trop bas et la commande a été acceptée. Je ne comprends pas comment une telle erreur peut être tolérée."
- Hypothèse d'amélioration : bloquer le paiement cash tant que le montant reçu est inférieur au total, avec un message explicite et une validation côté serveur.
- Priorité : Haute

### Utilisateur 5 — Camille, 31 ans, chef de projet UX, cliente occasionnelle
- Profil : très sensible à l'ergonomie et à la clarté des messages, utilise l'application une fois par mois.
- Contexte : consulte les restaurants et les promos sur mobile avant de commander.
- Problème : le panier et le checkout ne reflètent pas clairement les règles métier d'un restaurant (minimum d'achat, fermeture, stock).
- Fréquence : occasionnelle.
- Impact : doute sur la fiabilité du service ; demande de support pour comprendre les restrictions.
- Verbatim : "Je vois le restaurant, j’ajoute un produit, mais je n’ai pas de message pour savoir qu’il y a un minimum de commande ou que le restaurant est fermé."
- Hypothèse d'amélioration : intégrer des messages de validation au moment de l'ajout au panier et du paiement, avec règles métier visibles avant la confirmation.
- Priorité : Haute

### Utilisateur 6 — Paul, 38 ans, consultant, commande 1 à 2 fois par semaine
- Profil : client habitué aux systèmes de livraison, valorise la rapidité et la sécurité.
- Contexte : commande depuis le web en milieu de journée.
- Problème : le système ne protège pas les données d'un utilisateur dans les appels API, ce qui remet en cause la confiance dans les données exposées.
- Fréquence : régulière.
- Impact : perception de risque important sur les données personnelles et sur l'intégrité du service.
- Verbatim : "Si un appel API peut afficher des commandes d'un autre utilisateur, je ne peux pas avoir confiance dans cette application."
- Hypothèse d'amélioration : sécuriser les accès API avec authentification utilisateur réelle et contrôle strict des ressources demandées.
- Priorité : Critique
