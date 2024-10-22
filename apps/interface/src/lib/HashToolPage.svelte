<script lang="ts">
  import { lib } from "$lib";
  import type { Fr } from "@aztec/aztec.js";
  import { Ui } from "@repo/ui";
  import { utils } from "@repo/utils";
  import { z } from "zod";

  let {
    name,
    computeHash,
  }: {
    name: string;
    computeHash: (data: Fr[]) => Promise<Fr>;
  } = $props();

  let result: string | undefined = $state();
</script>

<svelte:head>
  <title>{name} - {lib.APP_NAME}</title>
</svelte:head>

<Ui.GapContainer class="container">
  <Ui.Card.Root>
    <Ui.Card.Header>
      <Ui.Card.Title class="mb-2">Compute {name}</Ui.Card.Title>
    </Ui.Card.Header>

    <Ui.Card.Content>
      <Ui.GapContainer class="gap-4">
        <Ui.Form
          schema={z.object({
            data: z.string().min(1),
          })}
          onsubmit={async (data) => {
            result = undefined;
            const { Fr } = await import("@aztec/aztec.js");
            const input = utils
              .removeSuffix(data.data.trim(), ",")
              .split(",")
              .map((s) => {
                const cleanS = s.trim().replace(/"/g, "").replace(/'/g, "");
                try {
                  return new Fr(BigInt(cleanS));
                } catch (e) {
                  throw new Error(
                    `Invalid number: "${cleanS}". Original error: ${e}`,
                  );
                }
              });
            result = (await computeHash(input)).toString();
          }}
        >
          {#snippet children(form, formData)}
            <Ui.Form.Field {form} name="data">
              <Ui.Form.Control let:attrs>
                <Ui.Form.Label>
                  Data (comma separated decimal or 0x-prefixed numbers)
                </Ui.Form.Label>
                <Ui.Textarea
                  {...attrs}
                  bind:value={formData.data}
                  placeholder="0x123, 0x456"
                />
              </Ui.Form.Control>
              <Ui.Form.Description></Ui.Form.Description>
              <Ui.Form.FieldErrors />
            </Ui.Form.Field>

            <Ui.Form.SubmitButton variant="default">
              Compute
            </Ui.Form.SubmitButton>
          {/snippet}
        </Ui.Form>

        {#if result}
          <Ui.CopyableInput text={result} />
        {/if}
      </Ui.GapContainer>
    </Ui.Card.Content>
  </Ui.Card.Root>
</Ui.GapContainer>
