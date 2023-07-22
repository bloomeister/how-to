# Incremental Static Regeneration (ISR)

## The Problem With Static-Site Generation

The idea behind the Jamstack is appealing: pre-rendered static pages which can be pushed to a CDN and globally available in seconds. Static content is fast, resilient to downtime, and immediately indexed by crawlers. But there are some issues.

**If you’ve adopted the Jamstack architecture while building a large-scale static site, you might be stuck waiting hours for your site to build. If you double the number of pages, the build time also doubles**

Even if every page was statically generated in an unrealistic 1ms, it would still take hours to rebuild the entire site. For large web applications, choosing complete static-site generation is a non-starter. Large-scale teams need a more flexible, personalized, hybrid solution.

## Content Management Systems (CMS)

For many teams, their site’s content is decoupled from the code. Using a **Headless CMS** allows content editors to publish changes without involving a developer. However, with traditional static sites, this process can be slow.

Consider an e-commerce store with 100,000 products. Product prices change frequently. When a content editor changes the price of headphones from $100 to $75 as part of a promotion, their CMS uses a webhook to rebuild the entire site. It’s not feasible to wait hours for the new price to be reflected.

Long builds with unnecessary computation might also incur additional expenses. Ideally, your application is intelligent enough to understand which products changed and incrementally update those pages without needing a full rebuild.

## Incremental Static Regeneration (ISR)

Next.js allows you to create or update static pages after you’ve built your site. Incremental Static Regeneration (ISR) enables developers and content editors to use static-generation on a per-page basis, without needing to rebuild the entire site. With ISR, you can retain the benefits of static while scaling to millions of pages.

Static pages can be generated at runtime (on-demand) instead of at build-time with ISR. Using analytics, A/B testing, or other metrics, you are equipped with the flexibility to make your own tradeoff on build times.

Consider the e-commerce store from before with 100,000 products. At a realistic 50ms to statically generate each product page, this would take almost 2 hours without ISR. With ISR, we can choose from:

    1. Faster Builds
    Generate the most popular 1,000 products at build-time. Requests made to other products will be a cache miss and statically generate on-demand: 1-minute builds.

    2. Higher Cache Hit Rate
    Generate 10,000 products at build-time, ensuring more products are cached ahead of a user’s request: 8-minute builds.

Let’s walk through an example of ISR for an e-commerce product page.

## Getting Started

### FETCHING DATA

    If you’ve never used Next.js before, I’d recommend reading Getting Started With Next.js to understand the basics. ISR uses the same Next.js API to generate static pages: getStaticProps. By specifying revalidate: 60, we inform Next.js to use ISR for this page.

    1. Next.js can define a revalidation time per page. Let’s set it at 60 seconds.
    2. The initial request to the product page will show the cached page with the original price.
    3. The data for the product is updated in the CMS.
    4. Any requests to the page after the initial request and before 60 seconds are cached and instantaneous.
    5. After the 60-second window, the next request will still show the cached (stale) page. Next.js triggers a regeneration of the page in the background.
    6. Once the page has been successfully generated, Next.js will invalidate the cache and show the updated product page. If the background regeneration fails, the old page remains unaltered.

```
// pages/products/[id].js

export async function getStaticProps({ params }) {
    return {
        props: {
            product: await getProductFromDatabase(params.id)
        },
        revalidate: 60
    }
}
```

### GENERATING PATHS

Next.js defines which products to generate at build-time and which on-demand. Let’s only generate the most popular 1,000 products at build-time by providing getStaticPaths with a list of the top 1,000 product IDs.

We need to configure how Next.js will “fallback” when requesting any of the other products after the initial build. There are two options to choose from: blocking and true.

    fallback: blocking // preferred

When a request is made to a page that hasn’t been generated, Next.js will server-render the page on the first request. Future requests will serve the static file from the cache.

    fallback: true

When a request is made to a page that hasn’t been generated, Next.js will immediately serve a static page with a loading state on the first request. When the data is finished loading, the page will re-render with the new data and be cached. Future requests will serve the static file from the cache.

```
// pages/products/[id].js

export async function getStaticPaths() {
    const products = await getTop1000Products()
    const paths = products.map((product) => ({
        params: { id: product.id }
    }))

    return { paths, fallback: 'blocking' }
}
```

## Tradeoffs

Next.js focuses first and foremost on the end-user. The “best solution” is relative and varies by industry, audience, and the nature of the application. By allowing developers to shift between solutions without leaving the bounds of the framework, Next.js lets you pick the right tool for the project.

### SERVER-SIDE RENDERING

ISR isn’t always the right solution. For example, the Facebook news feed cannot show stale content. In this instance, you’d want to use SSR and potentially your own cache-control headers with surrogate keys to invalidate content. Since Next.js is a hybrid framework, you’re able to make that tradeoff yourself and stay within the framework.

```
// You can cache SSR pages at the edge using Next.js
// inside both getServerSideProps and API Routes
res.setHeader('Cache-Control', 's-maxage=60, stale-while-revalidate');
```

SSR and edge caching are similar to ISR (especially if using stale-while-revalidate caching headers) with the main difference being the first request. With ISR, the first request can be guaranteed static if pre-rendered. Even if your database goes down, or there’s an issue communicating with an API, your users will still see the properly served static page. However, SSR will allow you to customize your page based on the incoming request.

> Note: Using SSR without caching can lead to poor performance. Every millisecond matters when blocking the user from seeing your site, and this can have a dramatic effect on your TTFB (Time to First Byte).

### STATIC-SITE GENERATION

ISR doesn’t always make sense for small websites. If your revalidation period is larger than the time it takes to rebuild your entire site, you might as well use traditional static-site generation.

### CLIENT-SIDE RENDERING

If you use React without Next.js, you’re using client-side rendering. Your application serves a loading state, followed by requesting data inside JavaScript on the client-side (e.g. useEffect). While this does increase your options for hosting (as there’s no server necessary), there are tradeoffs.

The lack of pre-rendered content from the initial HTML leads to slower and less dynamic Search Engine Optimization (SEO). It’s also not possible to use CSR with JavaScript disabled.

### ISR FALLBACK OPTIONS

If your data can be fetched quickly, consider using `fallback: blocking`. Then, you don’t need to consider the loading state and your page will always show the same result (regardless of whether it’s cached or not). If your data fetching is slow, `fallback: true` allows you to immediately show a loading state to the user.

### ISR: Not Just Caching!

While I’ve explained ISR through the context of a cache, it’s designed to persist your generated pages between deployments. This means that you’re able to roll back instantly and not lose your previously generated pages.

Each deployment can be keyed by an ID, which Next.js uses to persist statically generated pages. When you roll back, you can update the key to point to the previous deployment, allowing for atomic deployments. This means that you can visit your previous immutable deployments and they’ll work as intended.

-   Here’s an example of reverting code with ISR:
-   You push code and get a deployment ID 123.
-   Your page contains a typo “Smshng Magazine”.
-   You update the page in the CMS. No re-deploy needed.
-   Once your page shows “Smashing Magazine”, it’s persisted in storage.
-   You push some bad code and deploy ID 345.
-   You roll back to deployment ID 123.
-   You still see “Smashing Magazine”.

Reverts and persisting static pages are out of scope of Next.js and dependent on your hosting provider. Note that ISR differs from server-rendering with Cache-Control headers because, by design, caches expire. They are not shared across regions and will be purged when reverting.

## Examples Of Incremental Static Regeneration

Incremental Static Regeneration works well for e-commerce, marketing pages, blog posts, ad-backed media, and more.

## Learn Next.Js Today

Developers and large teams are choosing Next.js for its hybrid approach and ability to incrementally generate pages on-demand. With ISR, you get the benefits of static with the flexibility of server-rendering. ISR works out of the box using next start.

Next.js has been designed for gradual adoption. With Next.js, you can continue using your existing code and add as much (or as little) React as you need. By starting small and incrementally adding more pages, you can prevent derailing feature work by avoiding a complete rewrite. Learn more about Next.js — and happy coding, everyone!
