# O’Chat — Carnet de bord

## 🎯 Objectif
Mettre en place les bases de l’application de messagerie O’Chat, avec une interface responsive et les premières mécaniques d’interaction, en respectant les étapes demandées.

---

## ✅ Ce que j’ai mis en place

### Jour 1
- Initialisation du projet avec **Svelte + Vite**
- Mise en place de l’interface utilisateur de la zone de chat
- Découpage de l’UI en composants :
  - Header
  - Zone de messages
  - Zone de saisie
- Travail avec tailwind.
- Mise en place d’un layout de type “application” :
  - header et footer fixes
  - zone centrale scrollable


### Jour 2 (en cours)
- Mise en place d’un système de **saisie et stockage du token Mistral**
- Documentation et Utilisation du **localStorage** pour conserver le token côté navigateur
- Blocage de l’accès à l’application tant que le token n’est pas renseigné
- Mise en place de la communication enfant → parent via événements Svelte
- Début de la mécanique d’envoi de message (uniquement via console.log)

---

## ❌ Ce que je voulais faire mais que je n’ai pas encore terminé
- Affichage dynamique des messages dans la zone de chat
- Appel réel à l’API Mistral pour obtenir une réponse de l’IA
- Gestion de l’historique des messages
- Rendu du Markdown retourné par l’IA

---

## 🚧 Points bloquants / difficultés rencontrées
- Configuration initiale de l’environnement (Svelte 5, Tailwind, PostCSS)
- Compréhension du fonctionnement des événements Svelte et de `event.detail`
- Mise en place correcte du layout pour éviter le scroll global de la page

---

## 🔁 Ce que je referais différemment
- Prendre plus de temps au départ pour comprendre les différences entre Svelte 4 et Svelte 5

---

## ⏭️ Prochaines étapes
- Rendre la liste des messages dynamique
- Envoyer les messages utilisateur à l’API Mistral
- Afficher les réponses de l’IA dans le chat
---

## 🧠 Bilan
Les bases du projet sont posées.  
L’interface est fonctionnelle et prête à accueillir les interactions avec l’IA.  
Reste à brancher l’API
