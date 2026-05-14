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
