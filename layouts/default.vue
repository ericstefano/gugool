<script setup lang="ts">
const colorMode = useColorMode()
const isDarkMode = computed(() => colorMode.preference === 'dark')
function toggleColorMode() {
  isDarkMode.value ? colorMode.preference = 'light' : colorMode.preference = 'dark'
}
</script>

<template>
  <div class="flex flex-col p-4 mx-auto min-h-screen max-w-screen-md">
    <main class="flex flex-1">
      <slot />
    </main>
    <footer class="mt-2">
      <nav>
        <ul class="flex justify-center items-center">
          <li>
            <Button variant="ghost" size="icon" @click="toggleColorMode">
              <Icon size="1.25rem" :name="isDarkMode ? 'lucide:sun' : 'lucide:moon'" />
            </Button>
          </li>
        </ul>
      </nav>
    </footer>
    <Toaster position="top-center" />
  </div>
</template>
