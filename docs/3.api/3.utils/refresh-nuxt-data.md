---
title: 'refreshNuxtData'
description: refreshNuxtData refetches all data from the server and updates the page.
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/asyncData.ts
    size: xs
---

::note
`refreshNuxtData` re-fetches all data from the server and updates the page as well as invalidates the cache of [`useAsyncData`](/docs/api/composables/use-async-data) , `useLazyAsyncData`, [`useFetch`](/docs/api/composables/use-fetch) and `useLazyFetch`.
::

## Type

```ts
refreshNuxtData(keys?: string | string[])
```

**Parameters:**

* `keys`:

    **Type**: `String | String[]`

    `refreshNuxtData` accepts a single or an array of strings as `keys` that are used to fetch the data. This parameter is **optional**. All [`useAsyncData`](/docs/api/composables/use-async-data) and [`useFetch`](/docs/api/composables/use-fetch) are re-fetched when no `keys` are specified.

## Refresh All Data

This example below refreshes all data being fetched using [`useAsyncData`](/docs/api/composables/use-async-data) and [`useFetch`](/docs/api/composables/use-fetch) on the current page.

```vue [pages/some-page.vue]
<script setup lang="ts">
const refreshing = ref(false)
const refreshAll = async () => {
  refreshing.value = true
  try {
    await refreshNuxtData()
  } finally {
    refreshing.value = false
  }
}
</script>

<template>
  <div>
    <button :disabled="refreshing" @click="refreshAll">
      Refetch All Data
    </button>
  </div>
</template>
```

## Refresh Specific Data

This example below refreshes only data where the key matches to `count`.

```vue [pages/some-page.vue]
<script setup lang="ts">
const { status, data: count } = await useLazyAsyncData('count', () => $fetch('/api/count'))
const refresh = () => refreshNuxtData('count')
</script>

<template>
  <div>
    {{ status === 'pending' ? 'Loading' : count }}
  </div>
  <button @click="refresh">Refresh</button>
</template>
```

:read-more{to="/docs/getting-started/data-fetching"}
