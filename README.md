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

### VII. Data Fetching

1. `useFetch`
   ```ts
   const { data, status, error, refresh, clear } =
     await useFetch("/api/modules");
   ```
2. `useAsyncData`: Gọi nhiều API cùng lúc, sử dụng thư viện bên thứ ba (như Firebase, Apollo), hoặc cần xử lý/lọc dữ liệu trước khi trả về
   ```ts
   const { data, status, error, refresh, clear } = await useAsyncData(
     "movies",
     () => $fetch("https://api.nuxtjs.dev/movies"),
   );
   ```
3. Các Options khác:
   - `lazy`: Cho phép chuyển trang ngay lập tức mà không đợi dữ liệu tải xong (kết hợp với status pending để hiển thị loading)
   - `server`: Có cho phép lấy dữ liệu ở phía server hay không (mặc định là true)
   - `watch`: Tự động gọi lại dữ liệu khi một biến reactive thay đổi (ví dụ: thay đổi số trang trong phân trang)
   - `pick`: Chỉ lấy một vài trường cụ thể từ kết quả API để giảm dung lượng tải
   - `transform`
   - `status`: idle, pending, success, error

### VIII. Layers

### IX. Layout

```ts
definePageMeta({
  layout: "admin", // Sử dụng file admin.vue
});
```

- Hoặc:

```ts
<template>
  <NuxtLayout name="admin">
    <p>Nội dung trang quản trị</p>
  </NuxtLayout>
</template>
```

- Hoặc:

```ts
setPageLayout("admin");
```

---

## Nuxt Advanced

1. Lazy loading
   - `<LazyVideoPlayer/>` thay vì `<VideoPlayer/>`
2. Client & Server Components:
   - `.client.vue`: Chỉ render ở phía trình duyệt (ví dụ: các chart, bản đồ cần đối tượng window)
   - `.server.vue`: Chỉ render ở phía máy chủ. Điều này giúp giảm lượng JS gửi xuống client vì logic của nó không cần chạy lại ở trình duyệt
