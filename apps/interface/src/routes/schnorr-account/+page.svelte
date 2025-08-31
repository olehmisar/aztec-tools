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
  <Ui.Card.Root>
    <Ui.Card.Header>
      <Ui.Card.Title class="mb-2">Create a Schnorr account</Ui.Card.Title>

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
          const { publicKeys } = await deriveKeys(encryptionKey);
          const { constructorArgs } =
            await accountContract.getDeploymentFunctionAndArgs();
          const instance = await getContractInstanceFromDeployParams(
            await accountContract.getContractArtifact(),
            {
              constructorArgs,
              salt,
              publicKeys,
            },
          );
          const completeAddress =
            await CompleteAddress.fromSecretKeyAndInstance(
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
    </Ui.Card.Header>

    {#if account}
      <Ui.Card.Content>
        <Ui.GapContainer class="gap-4">
          <div>
            Complete address
            <Ui.CopyableInput text={account.completeAddress.toString()} />
          </div>

          <div>
            Address
            <Ui.CopyableInput
              text={account.completeAddress.address.toString()}
            />
          </div>

          <div>
            Encryption key
            <Ui.CopyableInput text={account.encryptionKey.toString()} />
          </div>

          <div>
            Signing key
            <Ui.CopyableInput text={account.signingKey.toString()} />
          </div>

          <div>
            Salt
            <Ui.CopyableInput text={account.salt.toString()} />
          </div>
        </Ui.GapContainer>
      </Ui.Card.Content>
    {/if}
  </Ui.Card.Root>
</Ui.GapContainer>
