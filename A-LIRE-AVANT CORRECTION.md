# O’Chat — Carnet de bord

## Objectif
Mettre en place une application de messagerie avec une IA, en construisant progressivement l’interface, la logique applicative et la persistance des données.

---

## Ce que j’ai mis en place

### Jour 1
- Initialisation du projet avec Svelte et Vite
- Mise en place de l’interface utilisateur de la zone de chat
- Découpage de l’UI en composants (zone de messages, zone de saisie, header)
- Utilisation de Tailwind CSS
- Mise en place d’un layout de type application avec header et footer fixes et une zone centrale scrollable

---

### Jour 2
- Mise en place d’un système de saisie du token Mistral
- Stockage du token dans le localStorage
- Blocage de l’accès à l’application tant que le token n’est pas renseigné
- Compréhension et utilisation des événements Svelte (dispatch, event.detail)
- Implémentation de l’envoi de messages utilisateur
- Appel réel à l’API Mistral et récupération des réponses
- Ajout dynamique des messages utilisateur et IA dans l’interface
- Gestion simple de l’état de chargement pour éviter les doubles envois
- Rendu des réponses de l’IA en Markdown avec svelte-exmarkdown

---

### Jour 3
- Installation et lancement de PocketBase en local
- Création d’une collection `messages` pour stocker l’historique
- Configuration des règles d’accès en public (lecture et création)
- Mise en place d’une fonction dédiée à l’enregistrement des messages en base
- Sauvegarde des messages utilisateur et des réponses de l’IA dans PocketBase
- Chargement de l’historique des messages au démarrage de l’application
- Synchronisation entre l’état local de l’application et les données stockées en base

---

## Ce que je voulais faire mais que je n’ai pas encore terminé
- Gestion plus avancée des erreurs (API IA ou base de données)
- Amélioration de la gestion des états de chargement

---

## Points bloquants ou difficultés rencontrées
- Mise en place de PocketBase en local
- Compréhension du moment où sauvegarder les messages (avant ou après affichage)
- Distinction entre logique applicative et composants d’affichage

---

## Ce que je referais différemment
- Prendre plus de temps pour comprendre l’architecture globale avant d’implémenter la persistance

---

## Prochaines étapes
- Ajouter un système de conversations
- Associer les messages à une conversation
- Améliorer l’expérience utilisateur lors des erreurs

---

## Bilan
L’application est maintenant capable de communiquer avec une IA, d’afficher les messages dynamiquement et de conserver un historique persistant grâce à PocketBase.  

