<script lang="ts" context="module">
  /**
   * @type {import('@sveltejs/kit').Load}
   */
  export async function load({ fetch }) {
    return {
      props: {
        mixData: await fetch("/mixes.json").then((res) => res.json()),
      },
    };
  }
</script>

<script lang="ts">
  import HeadTags from "$components/head-tags/HeadTags.svelte";
  import TagsContainer from "$shared/components/tags-container/TagsContainer.svelte";
  import type { IMetaTagProperties } from "$models/interfaces/imeta-tag-properties.interface";
  import MixPost from "$shared/components/mix-post/MixPost.svelte";
  import Button from "$shared/ui/components/button/Button.svelte";
  export let mixData;
  /**
   * @type {IMetaTagProperties}
   */
  const metaData: Partial<IMetaTagProperties> = {
    title: "Mixes",
    description: "Mixes page",
    url: "/mixes",
    keywords: ["mixes"],
    searchUrl: "/mixes",
  };

  const tags = mixData
    .map((x) => x.tags)
    .filter((x) => x.trim())
    .reduce((a, b) => (a.includes(b) ? a : [...a, b]), []);

  let visible = mixData;

  let selectedTags = [];

  let searchValue = "";

  const handleTagClick = (tag) => {
    if (selectedTags.includes(tag)) {
      selectedTags = selectedTags.filter((x) => x !== tag);
    } else {
      selectedTags = [...selectedTags, tag];
    }
    visible = mixData.filter((x) => {
      if (selectedTags.length === 0) return true;
      return selectedTags.includes(x.tags);
    });
  };
  console.log("test");
</script>

<HeadTags {metaData} />
<div class="pt-[5%]">
  <div class="pl-[5%] pr-[10%] pt-[4%]">
    <h1
      class="font-bold text-2xl md:text-4xl tracking-tight mb-4 mt-8 text-black dark:text-white"
    >
      Mixes
    </h1>

    <div class="mb-4">
      <input
        bind:value={searchValue}
        aria-label="Search articles"
        type="text"
        placeholder="Search mixes"
        class="px-4 py-2 border border-gray-300 dark:border-gray-900 focus:ring-blue-500 focus:border-blue-500 block w-full rounded-md bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100"
      />
      <svg
        class="absolute right-3 top-3 h-5 w-5 text-gray-400 dark:text-gray-300"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width={2}
          d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
        />
      </svg>
    </div>

    <!-- <TagsContainer {tags} {visible}/> -->


    <div class="flex flex-row flex-wrap w-full mt-4 items-center">
      {#each tags as tag, index (tag)}
        <Button
          {tag}
          onClick={() => handleTagClick(tag)}
          active={selectedTags.includes(tag)}
        />
        {#if index !== tags.length - 1}
          <p class="mr-2 ml-2 text-black dark:text-white">
            {` • `}
          </p>
        {/if}
      {/each}
    </div>

    <div class="grid  grid-cols-4 grid-rows-4 justify-between gap-3">
      {#each visible as mixData}
        <MixPost {mixData} />
      {/each}
    </div>
  </div>
</div>
