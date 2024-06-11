<script setup lang="ts">
const search = ref('')
const hasSearch = computed(() => !!search.value.length)
async function unluckySearch() {
  await navigateTo(`https://google.com/search?q=${search.value}`, {
    external: true,
    open: {
      target: '_blank',
      windowFeatures: {
        noopener: true,
        noreferrer: true,
      },
    },
  })
}
async function searchTerm() {
  if (!hasSearch.value)
    return
  navigateTo(`/search?term=${search.value}`)
}
</script>

<template>
  <div class="w-full flex flex-col justify-center">
    <div>
      <div class="flex items-center gap-3 mb-4 justify-center">
        <img
          src="../public/logo.webp" class="size-16 invert saturate-100 rotate-180"
        >
        <p class="text-6xl select-none font-bold">
          Gugool
        </p>
      </div>
      <form @submit.prevent="searchTerm">
        <div class="relative w-full items-center mb-4 max-w-lg mx-auto">
          <Input id="search" v-model="search" type="text" placeholder="Busca..." class="pl-10 rounded-xl h-12 text-lg" />
          <span class="absolute start-0 inset-y-0 flex items-center justify-center pl-3 pr-4">
            <Icon class="size-5 text-muted-foreground" name="lucide:search" />
          </span>
        </div>
        <div class="flex justify-center gap-4 flex-wrap">
          <Button class="font-bold" type="button" variant="default" :disabled="!hasSearch" @click="searchTerm">
            Pesquisa Gugool
          </Button>
          <Button class="font-bold" variant="outline" type="button" :disabled="!hasSearch" @click="unluckySearch">
            Não estou com sorte
          </Button>
        </div>
      </form>
    </div>
  </div>
</template>
