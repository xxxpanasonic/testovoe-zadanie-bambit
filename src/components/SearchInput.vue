<template>
  <div class="search-input">
    <input v-model="albumId" placeholder="Номер альбома" @input="fetchPhotos" />

  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['update-photos', 'update-album-ids'])
const albumId = ref('')

const fetchPhotos = async () => {
  try {
    const url = albumId.value
      ? `https://jsonplaceholder.typicode.com/photos?${albumId.value.split(' ').map(id => `albumId=${id}`).join('&')}`
      : 'https://jsonplaceholder.typicode.com/photos'

    const response = await fetch(url)
    if (!response.ok) throw new Error('Ошибка JSON')
    const data = await response.json()
    emit('update-photos', data)
    emit('update-album-ids', albumId.value.split(' ').map(Number))
  } catch {
    console.log('err')
  }
}

</script>

<style scoped lang="sass">
.search-input
  display: flex
  gap: 10px
  margin-bottom: 20px

  input
    flex: 1
    padding: 8px
    border: 1px solid #ccc
    border-radius: 4px
</style>