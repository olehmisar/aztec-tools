<script lang="ts">
  import { lib } from "$lib";
  import { Ui } from "@repo/ui";

  let secretAndHash:
    | {
        secret: string;
        secretHash: string;
      }
    | undefined = $state();
</script>

<svelte:head>
  <title>Secret & hash - {lib.APP_NAME}</title>
</svelte:head>

<Ui.GapContainer class="container">
  <div class="prose">
    <h1>Secret & hash</h1>
  </div>

  <Ui.LoadingButton
    variant="default"
    onclick={async () => {
      const { Fr, computeSecretHash } = await import("@aztec/aztec.js");
      const secret = Fr.random();
      const secretHash = computeSecretHash(secret);
      secretAndHash = {
        secret: secret.toString(),
        secretHash: secretHash.toString(),
      };
    }}
  >
    Create secret & hash
  </Ui.LoadingButton>

  {#if secretAndHash}
    <section>
      <div>
        Secret
        <div class="flex gap-2">
          <Ui.Input value={secretAndHash.secret} />
          <Ui.CopyButton text={secretAndHash.secret} />
        </div>
      </div>

      <div>
        Secret hash
        <div class="flex gap-2">
          <Ui.Input value={secretAndHash.secretHash} />
          <Ui.CopyButton text={secretAndHash.secretHash} />
        </div>
      </div>
    </section>
  {/if}
</Ui.GapContainer>
