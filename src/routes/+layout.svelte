<script lang="ts">
  import { invalidate } from "$app/navigation";
  import { onMount } from "svelte";

  export let data;

  $: ({ supabase, session } = data);

  onMount(() => {
    const { data } = supabase.auth.onAuthStateChange((_, _session) => {
      if (_session?.expires_at !== session?.expires_at) {
        invalidate("supabase:auth");
      }
    });

    return () => data.subscription.unsubscribe();
  });
</script>

<svelte:head>
  <title>Supabase Auth Helpers Demo</title>
</svelte:head>

<slot />
