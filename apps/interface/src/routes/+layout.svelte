<script lang="ts">
  import "../app.css";

  import { dev } from "$app/environment";
  import { lib } from "$lib";
  import { Ui } from "@repo/ui";
  import { QueryClientProvider } from "@tanstack/svelte-query";
  import { inject } from "@vercel/analytics";
  import { onMount } from "svelte";
  import Header from "./Header.svelte";

  let { children } = $props();

  inject({ mode: dev ? "development" : "production" });

  onMount(async () => {
    // load bb.js WASM eagerly
    const { poseidon2Hash } = await import("@aztec/foundation/crypto");
    console.log("init", poseidon2Hash([]));
  });
</script>

<QueryClientProvider client={lib.queries.queryClient}>
  <div class="flex h-full flex-col">
    <Header>
      <div class="mt-3">
        {@render children()}
      </div>
    </Header>
  </div>

  <Ui.Toaster position="bottom-right" />
</QueryClientProvider>
