<script lang="ts">
  import { _ } from "svelte-i18n";
  import Icon from "@iconify/svelte";
  import dayjs from "dayjs";
  import type { Post } from "./modules/post";
  import { defaultAuthor } from "./modules/config";
  import UserContent from "./UserContent.svelte";
  import { hasLangMismatch, locale, locales } from "./modules/i18n";

  export let post: Post;

  const authors = [defaultAuthor];

  let published: string;
  $: (published = dayjs(post.published).fromNow()), $locale;
</script>

<article>
  <div class="mb-5 max-w-xl">
    <h1 class="text-3xl font-semibold">{post.title}</h1>
    {#if post.subtitle}
      <h2 class="text-xl detail">{post.subtitle}</h2>
    {/if}
    <h3 class="mt-2">
      <span>{$_("post.authored_by")}</span>
      {#each authors as author}
        <a href={author.url} class="link-emphasis">{author.name}</a>
      {/each}
      <span>{published}</span>
    </h3>
    {#if hasLangMismatch($locale, post)}
      <div class="mt-4 inline-block notice text-sm">
        <span aria-hidden="true">
          {locales[post.language].flag}
        </span>
        <span
          >{$_("post.lang_mismatch", {
            values: { language: $_(`locale.${post.language}`) },
          })}</span
        >
      </div>
    {/if}
  </div>
  <div class="font-light max-w-3xl">
    <UserContent content={post.content} />
  </div>
</article>
<div class="mt-8">
  <a href="/all" class="detail flex items-center gap-1">
    <span>{$_("post.view_more")}</span>
    <Icon class="inline" icon="lucide:arrow-right" width="24" />
  </a>
</div>
