<template>
  <div class="app">
    <SearchInput @update-photos="photos = $event" @update-album-ids="albumIds = $event" />
    <PhotoTable :photos="filteredPhotos" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import SearchInput from './components/SearchInput.vue'
import PhotoTable from './components/PhotoTable.vue'

const photos = ref([])
const albumIds = ref([])

const filteredPhotos = computed(() =>
  albumIds.value.length ? photos.value.filter(photo => albumIds.value.includes(photo.albumId)) : photos.value
)
</script>

<style lang="sass">
body
  margin: 0
  padding: 0
  box-sizing: border-box

.app
  padding: 20px
  font-family: Arial, sans-serif
</style>