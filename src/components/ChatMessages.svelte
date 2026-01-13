<script>
  import Markdown from "svelte-exmarkdown";
  import { gfmPlugin } from "svelte-exmarkdown/gfm";

  export let messages = [];

  const plugins = [gfmPlugin()];
</script>


<section class="min-h-0 flex-1 overflow-y-auto px-4 py-6 pb-28 space-y-6">

  {#if messages.length > 0}
    <div class="flex items-center gap-3">
      <div class="h-px flex-1 bg-white/10"></div>
      <span class="text-xs text-zinc-500">Aujourd’hui</span>
      <div class="h-px flex-1 bg-white/10"></div>
    </div>
  {/if}

  {#each messages as msg (msg.id)}

    {#if msg.role === "user"}
      <!-- Message utilisateur -->
      <div class="flex items-start gap-3 justify-end">
        <div>
          <div class="max-w-[85%] sm:max-w-[75%] rounded-2xl bg-indigo-600 px-4 py-3 text-sm leading-relaxed text-white">
            {msg.content}
          </div>
        </div>

        <div class="h-8 w-8 shrink-0 rounded-full bg-zinc-700 flex items-center justify-center text-xs font-bold">
          ME
        </div>
      </div>

    {:else}
      <!-- Message IA -->
      <div class="flex items-start gap-3 justify-start">
        <div class="h-8 w-8 shrink-0 rounded-full bg-indigo-500 flex items-center justify-center text-xs font-bold">
          AI
        </div>

        <div>
          <div class="max-w-[85%] sm:max-w-[75%] rounded-2xl border border-white/10 bg-white/10 px-4 py-3 text-sm leading-relaxed">
            <div class="markdown-body">
              <Markdown md={msg.content} {plugins} />
            </div>


          </div>
        </div>
      </div>
    {/if}

  {/each}
</section>
