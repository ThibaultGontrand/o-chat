<script>
  import { onMount } from "svelte";

  import ChatHeader from "./components/ChatHeader.svelte";
  import ChatMessages from "./components/ChatMessages.svelte";
  import ChatComposer from "./components/ChatComposer.svelte";
  import PocketBase from "pocketbase";

const pb = new PocketBase("http://127.0.0.1:8090");


  // Clé utilisée pour stocker le token dans le localStorage
  // (simple et conforme à ton choix)
  const TOKEN_KEY = "mistral_token";

  // Token réellement utilisé par l’application :
  // - s'il est vide => l'utilisateur n'a pas accès au chat
  // - s'il est rempli => l'utilisateur peut accéder à l'app
  let token = "";

  // Valeur tapée dans l'input (ce n'est pas encore "validé")
  let tokenInput = "";

  // Message d'erreur à afficher si token invalide
  let tokenError = "";

  // Message d'erreur à afficher si token invalide
  let messages = [];

  // Pour le loader/spinner et les erreurs API
  let isLoading = false;
 
  // onMount s’exécute une fois quand App.svelte est monté
  // Idéal pour lire localStorage (qui n'existe que dans le navigateur)
  onMount(() => {
    const storedToken = localStorage.getItem(TOKEN_KEY);
    if (storedToken) token = storedToken;
    loadMessagesFromDB();
  });

  // Enregistre le token :
  // - nettoie la saisie avec trim()
  // - refuse si vide
  // - stocke dans localStorage
  // - met à jour `token` => l'UI passe du "gate" au chat
  function saveToken() {
    tokenError = "";

    // trim() enlève les espaces au début et à la fin :
    // "   abc   " => "abc"
    // et "     " => "" (vide)
    const cleanedToken = tokenInput.trim();

    if (!cleanedToken) {
      tokenError = "Veuillez saisir un token Mistral valide.";
      return;
    }

    // Stocke le token dans le navigateur (localStorage)
// - TOKEN_KEY : le nom sous lequel on range la donnée ("mistral_token")
// - cleanedToken : la valeur à stocker (le token saisi par l'utilisateur)
    localStorage.setItem(TOKEN_KEY, cleanedToken);
    token = cleanedToken;
    tokenInput = "";
  }

  // Supprime le token du navigateur
  // (utile pour tester ou changer de token)
  // NOTE : removeItem est une méthode native du navigateur (Web API),
  // pas une fonction Svelte.
  function resetToken() {
    localStorage.removeItem(TOKEN_KEY);
    token = "";
    tokenInput = "";
    tokenError = "";
  }


  async function sendMessageToMistral(userMessage) {
  // 1. envoyer userMessage à Mistral
   // URL officielle de l’API Mistral (chat)
  console.log("🔑 Token présent ?", Boolean(token), "taille:", token?.length ?? 0);

  const url = "https://api.mistral.ai/v1/chat/completions";

  // On prépare la requête HTTP
  const response = await fetch(url, {
    method: "POST",

    headers: {
      // Indique qu’on envoie du JSON
      "Content-Type": "application/json",

      // Token stocké dans le localStorage
      // Format imposé par l’API : "Bearer <token>"
      Authorization: `Bearer ${token}`,
    },

    body: JSON.stringify({
      // Le modèle utilisé par l’IA
      model: "mistral-small-latest",


      // Messages envoyés à l’IA
      // Pour l’instant : UN SEUL message utilisateur
     messages: [
  { role: "system", content: "Réponds toujours en Markdown et utilise des listes Markdown avec des tirets '-'." },
  { role: "user", content: userMessage }
  ],
    }),
  });

 // Si l’API répond avec une erreur HTTP
  if (!response.ok) {
    throw new Error("Erreur lors de l'appel à l'API Mistral");
  }

  // On transforme la réponse en objet JS
  const data = await response.json();

  // On récupère UNIQUEMENT le texte généré par l’IA
  const aiText = data.choices[0].message.content;

  // On retourne le texte (string)
  return aiText;
 
}


async function sendMessage(text) {
  isLoading = true;

  // 1️⃣ MESSAGE USER — affichage immédiat
  const userMessage = {
    id: crypto.randomUUID(),
    role: "user",
    content: text,
  };

  messages = [...messages, userMessage];

  // 🔽 ICI : on sauvegarde le message USER en base
  await saveMessageToDB({
    role: "user",
    content: text,
  });

  // 2️⃣ APPEL À MISTRAL
  const aiText = await sendMessageToMistral(text);

  // 3️⃣ MESSAGE IA — affichage
  const aiMessage = {
    id: crypto.randomUUID(),
    role: "assistant",
    content: aiText,
  };

  messages = [...messages, aiMessage];

  // 🔽 ICI : on sauvegarde le message IA en base
  await saveMessageToDB({
    role: "assistant",
    content: aiText,
  });

  isLoading = false;
}



async function loadMessagesFromDB() {
  // récupère tous les messages, triés du plus ancien au plus récent
  const records = await pb.collection("messages").getFullList({
    sort: "created",
  });

  // on transforme les records PocketBase en format attendu parl'UI
  messages = records.map((r) => ({
    id: r.id,
    role: r.role,
    content: r.content,
  }));
}


async function saveMessageToDB({ role, content }) {
  await pb.collection("messages").create({
    role,
    content,
  });
}



</script>

<main class="relative min-h-screen bg-zinc-950 text-zinc-100">
  <!-- Glow / vignette (polish) -->
  <div class="pointer-events-none absolute inset-0">
    <div class="absolute -top-24 left-1/2 h-72 w-72 -translate-x-1/2 rounded-full bg-indigo-600/20 blur-3xl"></div>
    <div class="absolute inset-0 bg-gradient-to-b from-transparent via-zinc-950/40 to-zinc-950"></div>
  </div>

  {#if !token}

    <!-- Écran de saisie du token (bloque l'app tant que token absent) -->
    <div class="relative mx-auto flex h-dvh max-w-3xl items-center justify-center px-4">
      <div class="w-full max-w-md rounded-2xl border border-white/10 bg-white/5 p-5 shadow-xl">
        <h2 class="text-lg font-semibold">Ajouter votre token Mistral</h2>
        <p class="mt-1 text-sm text-zinc-400">
          Le token est stocké dans votre navigateur (LocalStorage) et n’apparaît pas dans le code.
        </p>

        <label class="mt-4 block text-sm text-zinc-300">Token</label>
        <input
          class="mt-2 w-full rounded-xl bg-zinc-950/60 px-4 py-3 text-sm outline-none ring-1 ring-white/10 focus:ring-2 focus:ring-indigo-500"
          placeholder="mistral_..."
          bind:value={tokenInput}
        />

        {#if tokenError}
          <p class="mt-2 text-sm text-red-400">{tokenError}</p>
        {/if}

        <button
          class="mt-4 w-full rounded-xl bg-indigo-600 px-4 py-3 text-sm font-semibold text-white hover:bg-indigo-500 active:bg-indigo-700"
          on:click={saveToken}
        >
          Enregistrer le token
        </button>

        <p class="mt-3 text-xs text-zinc-500">
          Astuce : tu peux changer de token plus tard via le bouton “Changer token”.
        </p>
      </div>
    </div>
  {:else}


    <!-- UI Chat (J1) accessible seulement si token présent -->
    <div
      class="relative mx-auto flex h-dvh max-w-3xl flex-col overflow-hidden
             sm:my-6 sm:h-[calc(100dvh-3rem)] sm:rounded-2xl sm:border sm:border-white/10 sm:bg-white/5 sm:shadow-xl"
    >
      <ChatHeader />
      <ChatMessages {messages} />
      <ChatComposer on:submitMessage={(e) => sendMessage(e.detail.text)}
      disabled={isLoading} />   <!-- Écoute l'événement dispatch() et appelle sendMessage -->

    </div>

    <!-- Bouton debug pour changer/supprimer le token -->
    <button
      class="fixed bottom-4 right-4 rounded-full bg-white/10 px-4 py-2 text-xs text-zinc-100 hover:bg-white/15"
      on:click={resetToken}
      title="Supprimer le token stocké"
    >
      Changer token
    </button>
  {/if}
</main>



