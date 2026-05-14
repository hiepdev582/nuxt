# Nuxt

## Nuxt Basics

### I. Configurations

1. Environment Overrides
   - `$production`
   - `$development`
   - `$env`: can specify custom name (e.g `$staging`)

2. `runtimeConfig`: Private or public tokens that need to be specified after build using environment variables

```ts
export default defineNuxtConfig({
  runtimeConfig: {
    // The private keys which are only available server-side
    apiSecret: '123',
    // Keys within public are also exposed client-side
    public: {
      apiBase: '/api',
    },
  },
})
// These variables are exposed to the rest of your application using the useRuntimeConfig() composable.
<script setup lang="ts">
const runtimeConfig = useRuntimeConfig()
</script>
```

3. `appConfig`:Public tokens that are determined at build time, website configuration such as theme variant, title and any project config that are not sensitive

```ts
export default defineAppConfig({
  title: 'App',
  theme: {
    dark: true,
    colors: {
      primary: '#ff0000',
    },
  },
})

<script setup lang="ts">
const appConfig = useAppConfig()
</script>
```

### II. Assets

1. `app/assets/`

```
<img src="~/assets/img/nuxt.png" />
```

2. CSS Property:

```ts
export default defineNuxtConfig({
  css: ["~/assets/css/main.css"], // Apply to all pages
});
```

3. Font: Place your local fonts files in your `public/` directory, for example `public/fonts`. You can then reference them in your stylesheets using `url()`

### III. Style

1. Dynamic Styles With `v-bind`

```ts
<script setup lang="ts">
const color = ref('red')
</script>

<template>
  <div class="text">
    hello
  </div>
</template>

<style>
.text {
  color: v-bind(color);
}
</style>

```

### IV. Routes

1. `definePageMeta`:
   - validate
   ```ts
   export default definePageMeta({
     validate: ({ params }) => {
       return !!params.slug;
     },
   });
   ```

### V. SEO and Meta

1. Customize the head for your entire app

```ts
export default defineNuxtConfig({
  app: {
    head: {
      title: "My Nuxt App",
      htmlAttrs: {
        lang: "en",
      },
      link: [{ rel: "icon", type: "image/x-icon", href: "/favicon.ico" }],
      meta: [{ name: "description", content: "My Nuxt App" }],
    },
  },
});
```

2. `useHead` / `useHeadSafe`:

```ts
<script setup lang="ts">
useHead({
  title: 'My App',
  meta: [
    { name: 'description', content: 'My amazing site.' },
  ],
})
</script>

```

3. `useSeoMeta`

```ts
<script setup lang="ts">
useSeoMeta({
  title: 'My Amazing Site',
  ogTitle: 'My Amazing Site',
  description: 'This is my amazing site, let me tell you all about it.',
  ogDescription: 'This is my amazing site, let me tell you all about it.',
  ogImage: 'https://example.com/image.png',
  twitterCard: 'summary_large_image',
})
</script>

```

4. `titleTemplate`, `templateParams`, `tagPosition`

### VI. Transitions

1. Global Transitions:

```ts
export default defineNuxtConfig({
  app: {
    pageTransition: { name: "page", mode: "out-in" },
  },
});
```

2. Layout Transitions:

```ts
export default defineNuxtConfig({
  app: {
    layoutTransition: { name: "layout", mode: "out-in" },
  },
});
```

3. Page Transitions:

```ts
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'rotate',
  },
   layoutTransition: {
    name: 'slide-in',
  },
})
</script>

```
