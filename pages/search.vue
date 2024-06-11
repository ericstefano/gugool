<script setup lang="ts">
import { toast } from 'vue-sonner'
import { useVirtualList } from '@vueuse/core'

function normalizeUrl(url: string) {
  const getLastIndex = url.lastIndexOf('?')
  const lastIndex = getLastIndex === -1 ? url.length : getLastIndex
  return url.substring(0, lastIndex)
}

const route = useRoute()
onMounted(() => {
  if (!route.query.term)
    navigateTo('/')
})

const { data: items } = await useFetch<string[]>('http://0.0.0.0:8000/seach', {
  params: {
    distance: route.query.distance ?? 2,
    term: route.query.term,
  },
  retry: false,
  onRequestError() {
    toast.error('Falha na busca, por favor tente novamente!')
  },
})

const { list, containerProps, wrapperProps } = useVirtualList(items.value!, {
  itemHeight: 108 + 16,
})
</script>

<template>
  <div class="w-full flex flex-col">
    <template v-if="items?.length">
      <div class="flex my-6 items-center">
        <NuxtLink to=".." class="mr-2">
          <Button size="sm" variant="ghost">
            <Icon class="mr-1" name="lucide:arrow-left" />
            Voltar
          </Button>
        </NuxtLink>
        <p class="text-2xl">
          Buscando por "{{ route.query.term }}"  - {{ items.length }} {{ items.length === 1 ? 'item encontrado' : 'itens encontrados' }}.
        </p>
      </div>
      <div v-bind="containerProps" class="h-[31rem] overflow-y-scroll border border-1 px-8 pb-2 pt-6 rounded-lg">
        <div class="flex flex-col space-y-4" v-bind="wrapperProps">
          <NuxtLink v-for="{ index, data: item } in list" :key="index" external class="truncate transition-all hover:text-blue-500 opacity-75 h-[6.75rem]" :to="item" target="_blank">
            <Card class="hover:bg-primary transition-all ease-out group select-none">
              <CardHeader class="pb-1.5">
                <CardTitle class="group-hover:text-background truncate text-xl select-none">
                  {{ normalizeUrl(item).substring(item.lastIndexOf('/') + 1, item.length).split('-').map((word) => word[0].toUpperCase() + word.substring(1, word.length)).join(' ') }}
                </CardTitle>
              </CardHeader>
              <CardContent class="truncate group-hover:text-background opacity-70 select-none">
                {{ item }}
              </CardContent>
            </Card>
          </NuxtLink>
        </div>
      </div>
    </template>
    <div v-else class="flex flex-col gap-2">
      <p class="text-center text-4xl">
        Nenhum item encontrado
      </p>
      <NuxtLink to=".." class="mx-auto">
        <Button size="sm" variant="ghost">
          <Icon class="mr-1" name="lucide:arrow-left" />
          Voltar
        </Button>
      </NuxtLink>
    </div>
  </div>
</template>
