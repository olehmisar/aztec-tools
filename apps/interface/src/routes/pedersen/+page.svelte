<script lang="ts">
  import HashToolPage from "$lib/HashToolPage.svelte";
</script>

<HashToolPage
  name="Pedersen hash"
  computeHash={async (data) => {
    const { pedersenHash } = await import("@aztec/foundation/crypto");
    return pedersenHash(data);
  }}
/>
