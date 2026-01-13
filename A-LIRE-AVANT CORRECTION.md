# O’Chat — Carnet de bord

##  Objectif
Mettre en place les bases de l’application de messagerie O’Chat, avec une interface responsive et les premières mécaniques d’interaction, en respectant les étapes demandées.

---

## Ce que j’ai mis en place

### Jour 1
- Initialisation du projet avec **Svelte + Vite**
- Mise en place de l’interface utilisateur de la zone de chat
- Découpage de l’UI en composants :
  - Header
  - Zone de messages
  - Zone de saisie
- Travail avec **Tailwind CSS** pour la mise en forme
- Mise en place d’un layout de type “application” :
  - header et footer fixes
  - zone centrale scrollable
- Attention portée au responsive (desktop / mobile)

---

### Jour 2
- Mise en place d’un système de **saisie et stockage du token Mistral**
- Utilisation et compréhension du **localStorage** pour conserver le token côté navigateur
- Blocage de l’accès à l’application tant que le token n’est pas renseigné
- Mise en place de la communication **enfant → parent** via les événements Svelte (`dispatch`, `event.detail`)
- Implémentation de l’envoi de messages utilisateur
- Appel réel à l’API **Mistral AI** et récupération des réponses
- Affichage dynamique des messages utilisateur et IA dans la zone de chat
- Gestion simple de l’état de chargement pour éviter les doubles envois
- Mise en place du rendu **Markdown** des réponses de l’IA avec `svelte-exmarkdown`
- Utilisation du CSS GitHub Markdown pour un rendu propre et lisible dans les bulles de discussion

---

##  Ce que je voulais faire mais que je n’ai pas encore terminé
- Gestion de l’historique des messages sur une vraie base de données
- Système de conversations multiples
- Gestion plus avancée des erreurs API.

---

##  Points bloquants / difficultés rencontrées
- Configuration initiale de l’environnement (Svelte 5, Tailwind, PostCSS)
- Différences de syntaxe et de fonctionnement entre Svelte 4 et Svelte 5
- Compréhension du fonctionnement des événements personnalisés Svelte
- Mise en place du rendu Markdown dans un contexte de bulle de chat

---

##  Ce que je referais différemment
- Prendre plus de temps au départ pour bien comprendre l’écosystème Svelte 5
- Anticiper davantage la gestion des états (chargement, erreurs)

---

## Prochaines étapes
- Mettre en place la persistance des données (messages, conversations)
- Améliorer la gestion des erreurs 

---

##  Bilan
Les bases du projet sont maintenant solides.  
L’interface est fonctionnelle, l’API Mistral est correctement intégrée et les réponses sont affichées dynamiquement avec un rendu Markdown propre.  

