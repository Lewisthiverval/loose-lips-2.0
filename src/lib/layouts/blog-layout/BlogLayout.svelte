<script lang="ts">
  import { onMount } from "svelte";

  // Environment
  import { environment } from "$environment/environment";

  // Models

  // Components
  import HeadTags from "$components/head-tags/HeadTags.svelte";
  //   import ExternalLink from "$ui/components/external-link/ExternalLink.svelte";
  //   import ShareButtons from "$ui/components/share-buttons/ShareButtons.svelte";
  //   import NextArticle from "$ui/components/next-article/NextArticle.svelte";
  //   import TagsContainer from "$ui/components/tags-container/TagsContainer.svelte";
  import RecommendedPostContainer from "$lib/shared/components/recommended-posts/RecommendedPostsContainer.svelte";

  // Utils
  import { blogTypeDate } from "$utils/date-formatters";
  import { readingTime } from "$lib/utils/reading-time";
  import RecentPostsContainer from "$lib/shared/components/recent-posts-container/RecentPostsContainer.svelte";
  import RecommendedPost from "$lib/shared/components/recommended-post/RecommendedPost.svelte";
  //   import RecommendedPostsContainer from "$shared/components/recommended-posts/RecommendedPostsContainer.svelte";
  //   import { blogTags } from "$lib/data/tags";
  export let blogs;
  // Exports
  export let tags = [];
  export let title = "";
  export let slug = "";
  export let description = "";
  // export let tags = [];
  export let date = "";
  export let author = "";
  //   export let previousArticleLink = "";
  //   export let nextArticleLink = "";

  let readingTimeDuration = "";

  //   const editUrl = `${environment.gitHubConfig.GITHUB_BLOG_EDIT_URL}/${slug}/index.md`;
  //   const discussUrl = `${
  //     environment.twitterConfig.TWITTER_SEARCH_URL
  //   }?q=${encodeURIComponent(
  //     `https://sveltekit-blog-starter.vercel.app/blog/${slug}`
  //   )}`;

  /**
   * @type {IMetaTagProperties}
   */
  let metaData = {
    title: `${title} | Sveltekit`,
    description: `${description}`,
    url: `/blog/${slug}`,
    keywords: [
      "sveltekit blog",
      "sveltekit starter",
      "svelte starter",
      "svelte",
      ...tags,
    ],
    tags: tags,
    searchUrl: `/blog/${slug}`,
    image: `/images/blogs/${slug}/banner.jpg`,
    twitter: {
      label1: "Written by",
      data1: author,
      label2: "Published on",
      data2: blogTypeDate(date),
    },
    openGraph: {
      type: "article",
    },
  };

  // Start: Reactive properties
  $: {
    if (title && slug) {
      metaData = {
        title: `${title} | Sveltekit`,
        url: `/blog/${slug}`,
        keywords: [
          "sveltekit blog",
          "sveltekit starter",
          "svelte starter",
          "svelte",
          ...tags,
        ],
        tags: tags,
        searchUrl: `/blog/${slug}`,
        description: `${description}`,
        image: `/images/blogs/${slug}/banner.jpg`,
        twitter: {
          label1: "Written by",
          data1: author,
          label2: "Published on",
          data2: blogTypeDate(date),
        },
        openGraph: {
          type: "article",
        },
      };
    }
  }

  console.log(title, "title");
  console.log(tags, "tags");

  // End: Reactive properties

  // Local Methods
  //   onMount(() => {
  //     readingTimeDuration = readingTime(
  //       `${document.getElementById("blog-conent").textContent}`
  //     ).time;
  //   });
</script>

<!-- Start: Header Tag -->
<HeadTags {metaData} />
<!-- End: Header Tag -->

<article
  class="flex flex-col justify-center items-start max-w-2xl mx-auto mb-16 w-full"
>
  <!-- <h1
    class="font-bold text-3xl md:text-5xl tracking-tight mb-4 text-black dark:text-white"
  >
    {title}
  </h1>
  <div
    class="flex flex-col md:flex-row justify-between items-start md:items-center w-full mt-2"
  > -->
  <!-- <div class="flex items-center">
      <img
        alt={"Sveltekit Blogger"}
        src={"/images/author/favicon-32x32.png"}
        class="rounded-full w-7 h-7"
      />
      <p class="text-sm text-gray-700 dark:text-gray-300 ml-2">
        {author}
        {" / "}
        {blogTypeDate(date)}
      </p>
    </div>
    <p class="text-sm text-gray-500 min-w-32 mt-2 md:mt-0">
      {readingTimeDuration}
    </p>
  </div> -->
  <div class="prose dark:prose-dark max-w-none w-full" id="blog-conent">
    <slot />
  </div>

  <!-- <div class="mt-8">
    <RecommendedPostsContainer {tags} />
    <TagsContainer {tags} />
  </div> -->
  <!-- <div class="mt-8">
    <p class="text-sm text-gray-700 dark:text-gray-300 mb-4">
      {"Share the article on"}
    </p>
    <ShareButtons
      {title}
      {description}
      url={`${environment.launchURL}/blog/${slug}`}
    />
  </div> -->
  <!-- <div class="text-sm text-gray-700 dark:text-gray-300 mt-8">
    <ExternalLink
      href={discussUrl}
      ariaLabel={title}
      cssClasses={"text-sm text-gray-700 dark:text-gray-300 hover:text-gray-900 dark:hover:text-gray-500"}
    >
      {"Discuss on Twitter"}
    </ExternalLink>
    {` • `}
    <ExternalLink
      href={editUrl}
      ariaLabel={title}
      cssClasses={"text-sm text-gray-700 dark:text-gray-300 hover:text-gray-900 dark:hover:text-gray-500"}
    >
      {"Edit on GitHub"}
    </ExternalLink>
  </div> -->
  <!-- {#if previousArticleLink || nextArticleLink}
    <div class="mt-8 w-full">
      <NextArticle
        previousHref={previousArticleLink}
        nextHref={nextArticleLink}
      />
    </div>
  {/if} -->
  <!-- </div> -->
</article>
<div class="flex flex-col justify-center items-start  mx-auto mb-16 w-full">
  <RecommendedPostContainer {tags} />
</div>
