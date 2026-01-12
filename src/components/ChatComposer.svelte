<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let inputValue = "";

  function handleSubmit() {
    const text = inputValue.trim();
    if (!text) return;

    // On envoie le texte au parent (App.svelte)
    dispatch("submitMessage", { text });

    inputValue = "";
  }
</script>

<footer class="border-t border-white/10 bg-zinc-950/80 px-4 py-4 backdrop-blur">
  <form class="flex items-end gap-3" on:submit|preventDefault={handleSubmit}>
    <textarea
      class="min-h-[44px] max-h-32 w-full resize-none rounded-xl bg-white/5 px-4 py-3 text-sm outline-none ring-1 ring-white/10 focus:ring-2 focus:ring-indigo-500"
      placeholder="Écris un message…"
      rows="1"
      bind:value={inputValue}
    ></textarea>

    <button
      type="submit"
      class="rounded-xl bg-indigo-600 px-4 py-3 text-sm font-semibold text-white hover:bg-indigo-500 active:bg-indigo-700"
    >
      Envoyer
    </button>
  </form>

  <p class="mt-2 text-xs text-zinc-500">
    Entrée pour envoyer • Shift+Entrée pour aller à la ligne
  </p>
</footer>
