<script lang="ts">
  import HashToolPage from "$lib/HashToolPage.svelte";
</script>

<HashToolPage
  name="Poseidon2 hash"
  computeHash={async (data) => {
    const { poseidon2Hash } = await import("@aztec/foundation/crypto");
    return poseidon2Hash(data);
  }}
/>
