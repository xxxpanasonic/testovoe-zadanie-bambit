<template>
  <div class="app">
    <SearchInput @update-photos="handleUpdatePhotos" @update-album-ids="handleUpdateAlbumIds" />
    <PhotoTable :photos="filteredPhotos" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import SearchInput from './components/SearchInput.vue'
import PhotoTable from './components/PhotoTable.vue'

const photos = ref([])
const albumIds = ref([])

const handleUpdatePhotos = (newPhotos) => {
  photos.value = newPhotos
}

const handleUpdateAlbumIds = (newAlbumIds) => {
  albumIds.value = newAlbumIds
}

const filteredPhotos = computed(() => {
  if (albumIds.value.length === 0) return photos.value
  return photos.value.filter(photo => albumIds.value.includes(photo.albumId))
})
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