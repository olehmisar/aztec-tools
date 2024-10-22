<script lang="ts">
  import { lib } from "$lib";
  import type { CompleteAddress, Fq, Fr } from "@aztec/aztec.js";
  import { Ui } from "@repo/ui";

  let account:
    | Readonly<{
        encryptionKey: Fr;
        signingKey: Fq;
        salt: Fr;
        completeAddress: CompleteAddress;
      }>
    | undefined = $state();
</script>

<svelte:head>
  <title>Schnorr account - {lib.APP_NAME}</title>
</svelte:head>

<Ui.GapContainer class="container">
  <div class="prose">
    <h1>Create a Schnorr account</h1>
  </div>

  <Ui.LoadingButton
    variant="default"
    onclick={async () => {
      const {
        CompleteAddress,
        getContractInstanceFromDeployParams,
        Fq,
        Fr,
        deriveKeys,
      } = await import("@aztec/aztec.js");
      const { SchnorrAccountContract } = await import(
        "@aztec/accounts/schnorr"
      );
      const signingKey = Fq.random();
      const encryptionKey = Fr.random();
      const salt = Fr.random();
      const accountContract = new SchnorrAccountContract(signingKey);
      const publicKeysHash = deriveKeys(encryptionKey).publicKeys.hash();
      const instance = getContractInstanceFromDeployParams(
        accountContract.getContractArtifact(),
        {
          constructorArgs: accountContract.getDeploymentArgs(),
          salt,
          publicKeysHash,
        },
      );
      const completeAddress = CompleteAddress.fromSecretKeyAndInstance(
        encryptionKey,
        instance,
      );
      account = {
        encryptionKey,
        signingKey,
        salt,
        completeAddress,
      };
    }}
  >
    Create
  </Ui.LoadingButton>

  {#if account}
    <Ui.Card.Root>
      <Ui.Card.Header>
        <Ui.Card.Title>Created account</Ui.Card.Title>
      </Ui.Card.Header>

      <Ui.Card.Content>
        <div class="flex flex-col gap-4">
          <div>
            Complete address
            <div class="flex gap-2">
              <Ui.Input readonly value={account.completeAddress.toString()} />
              <Ui.CopyButton text={account.completeAddress.toString()} />
            </div>
          </div>

          <div>
            Address
            <div class="flex gap-2">
              <Ui.Input
                readonly
                value={account.completeAddress.address.toString()}
              />
              <Ui.CopyButton
                text={account.completeAddress.address.toString()}
              />
            </div>
          </div>

          <div>
            Encryption key
            <div class="flex gap-2">
              <Ui.Input readonly value={account.encryptionKey.toString()} />
              <Ui.CopyButton text={account.encryptionKey.toString()} />
            </div>
          </div>
          <div>
            Signing key
            <div class="flex gap-2">
              <Ui.Input readonly value={account.signingKey.toString()} />
              <Ui.CopyButton text={account.signingKey.toString()} />
            </div>
          </div>
          <div>
            Salt
            <div class="flex gap-2">
              <Ui.Input readonly value={account.salt.toString()} />
              <Ui.CopyButton text={account.salt.toString()} />
            </div>
          </div>
        </div>
      </Ui.Card.Content>
    </Ui.Card.Root>
  {/if}
</Ui.GapContainer>
