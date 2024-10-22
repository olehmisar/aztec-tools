<script lang="ts">
  import { cn } from "#lib/utils.js";
  import type { HTMLAttributes } from "svelte/elements";
  import { Ui } from "..";
  let {
    text,
    ...props
  }: HTMLAttributes<HTMLDivElement> & {
    text: string;
  } = $props();
</script>

<div {...props} class={cn("flex gap-2", props.class)}>
  <Ui.Input readonly value={text} />
  <Ui.CopyButton {text} />
</div>
