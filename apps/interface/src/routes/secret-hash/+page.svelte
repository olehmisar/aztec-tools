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
  <Ui.Card.Root>
    <Ui.Card.Header>
      <Ui.Card.Title class="mb-2">Create secret & hash</Ui.Card.Title>
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
    </Ui.Card.Header>

    {#if secretAndHash}
      <Ui.Card.Content>
        <Ui.GapContainer class="gap-4">
          <div>
            Secret
            <Ui.CopyableInput text={secretAndHash.secret} />
          </div>

          <div>
            Secret hash
            <Ui.CopyableInput text={secretAndHash.secretHash} />
          </div>
        </Ui.GapContainer>
      </Ui.Card.Content>
    {/if}
  </Ui.Card.Root>
</Ui.GapContainer>
