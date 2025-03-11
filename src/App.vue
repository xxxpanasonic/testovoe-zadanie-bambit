<template>
  <div class="app" :class="{ 'dark': isDark }">
    <SearchInput @update-photos="photos = $event" />
    <PhotoTable :photos="filteredPhotos" @choose-dark="DarkMode" />
  </div>
</template>
<script setup>
import { ref, computed } from 'vue'
import SearchInput from './components/SearchInput.vue'
import PhotoTable from './components/PhotoTable.vue'

const photos = ref([])
const albumIds = ref([])
const isDark = ref(false)

const DarkMode = () => {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.body.classList.add('dark')
  } else {
    document.body.classList.remove('dark')
  }
}
const filteredPhotos = computed(() =>
  albumIds.value.length ? photos.value.filter(photo => albumIds.value.includes(photo.albumId)) : photos.value
)
</script>

<style lang="sass">
body
  margin: 0
  padding: 0
  width: 100%
  height: 100%
  &.dark
    background-color: #121212
    color: #ffffff
  .app
    padding: 20px
    font-family: Arial, sans-serif

    &.dark
      background-color: #121212
      color: #ffffff

      .photo-table
        &::darkMode
          color: #eee
          background: #121212

        table
          th, td
            border-color: #444

          th
            background-color: #333
            color: #fff

          td
            color: #fff
</style>