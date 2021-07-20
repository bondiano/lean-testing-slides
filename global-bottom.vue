<template>
  <footer
    v-if="$slidev.nav.currentLayout !== 'cover' && $slidev.nav.currentPage !== 1"
    class="absolute bottom-0 left-0 right-0 p-2"
  >
    {{ $slidev.nav.currentPage - 1 }} / {{ $slidev.nav.total - 1 }}
  </footer>
</template>
