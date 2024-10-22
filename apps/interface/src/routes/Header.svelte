<script lang="ts">
  import { page } from "$app/stores";
  import { lib } from "$lib";
  import { route } from "$lib/ROUTES";
  import { Ui } from "@repo/ui";
  import { utils } from "@repo/utils";
  import {
    BookLock,
    Github,
    Menu,
    MessageCirclePlus,
    Send,
    User,
  } from "lucide-svelte";
  import type { Snippet } from "svelte";

  let {
    children,
  }: {
    children?: Snippet;
  } = $props();

  function isActive(href: string) {
    if (href === "/") {
      return $page.url.pathname === "/";
    }
    return $page.url.pathname.startsWith(href);
  }

  const githubUrl = "https://github.com/olehmisar/aztec-tools";

  const links = [
    { text: "Create an account", href: route("/schnorr-account"), Icon: User },
    { text: "Secret hash", href: route("/secret-hash"), Icon: BookLock },
    {
      text: "Request a tool",
      href: utils.joinUrl(githubUrl, "/issues/new"),
      target: "_blank",
      Icon: MessageCirclePlus,
    },
  ];
</script>

<header
  class="sticky top-0 flex h-16 items-center gap-4 border-b bg-background px-4 md:px-6"
>
  <Ui.Sheet.Root>
    <Ui.Sheet.Trigger asChild let:builder>
      <Ui.Button
        variant="outline"
        size="icon"
        class="shrink-0 md:hidden"
        builders={[builder]}
      >
        <Menu class="h-5 w-5" />
        <span class="sr-only">Toggle navigation menu</span>
      </Ui.Button>
    </Ui.Sheet.Trigger>
    <nav class="flex flex-row items-center gap-5 text-sm font-medium">
      {@render navbar()}
    </nav>
    <Ui.Sheet.Content side="left">
      <nav class="grid gap-6 text-lg font-medium">
        {@render navbar()}
        {@render tools()}
      </nav>
    </Ui.Sheet.Content>
  </Ui.Sheet.Root>
  <div class="flex w-full items-center gap-1 md:ml-auto md:w-auto">
    <div class="grow"></div>
    {@render link({
      title: "GitHub",
      href: githubUrl,
      target: "_blank",
      Icon: Github,
    })}
    {@render link({
      title: "Contact developers on Telegram",
      href: "https://t.me/foustling",
      target: "_blank",
      Icon: Send,
    })}
  </div>
</header>

<div class="grid h-full grid-cols-12">
  <aside class="col-span-4 hidden border-r p-4 md:block">
    <nav>
      {@render tools()}
    </nav>
  </aside>

  <main class="col-span-full md:col-span-8">
    {@render children?.()}
  </main>
</div>

{#snippet navbar()}
  <a href={route("/")} class="flex items-center gap-2 font-semibold">
    <img src="/logo.png" alt="logo" class="size-[2em]" />
    {lib.APP_NAME}
  </a>
{/snippet}

{#snippet tools()}
  {#each links as l}
    {@render link(l)}
  {/each}
{/snippet}

{#snippet link({
  text,
  Icon,
  ...props
}: {
  text?: string;
  href: string;
  target?: string;
  title?: string;
  Icon: any;
})}
  <Ui.Button
    {...props}
    variant="ghost"
    class={Ui.cn(
      "flex items-center justify-start gap-2",
      isActive(props.href) ? "text-foreground" : "text-muted-foreground",
    )}
  >
    <Icon />
    {text ?? ""}
  </Ui.Button>
{/snippet}
