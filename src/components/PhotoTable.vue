<template>
  <div class="photo-table" @scroll="handleScroll">
    <table>
      <thead>
        <tr>
          <th @click="sort('id')">ID</th>
          <th @click="sort('albumId')">Альбом</th>
          <th @click="sort('title')">Название</th>
          <th @click="sort('url')">Ссылка</th>
          <th @click="sort('thumbnailUrl')">Миниатюра</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="photo in visiblePhotos" :key="photo.id">
          <td v-for="key in ['id', 'albumId', 'title', 'url', 'thumbnailUrl']" :key="key" :title="photo[key]">
            {{ photo[key] }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

// eslint-disable-next-line vue/require-default-prop
const props = defineProps({ photos: Array })
const visiblePhotos = ref([])
const sortKey = ref('id')
const sortDirection = ref('asc')

const sortedPhotos = computed(() =>
  props.photos ? [...props.photos].sort((a, b) =>
    (a[sortKey.value] > b[sortKey.value] ? 1 : -1) * (sortDirection.value === 'asc' ? 1 : -1)
  ) : []
)

const sort = (key) => {
  if (sortKey.value === key) sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc'
  // eslint-disable-next-line @typescript-eslint/no-unused-expressions
  else sortKey.value = key, sortDirection.value = 'asc'
  visiblePhotos.value = sortedPhotos.value.slice(0, 30)
}

const handleScroll = (event) => {
  const { scrollTop, scrollHeight, clientHeight } = event.target
  if (scrollTop + clientHeight >= scrollHeight - 50) loadMore()
}

const loadMore = () => {
  const currentLength = visiblePhotos.value.length
  visiblePhotos.value = [...visiblePhotos.value, ...sortedPhotos.value.slice(currentLength, currentLength + 30)]
}

onMounted(() => visiblePhotos.value = sortedPhotos.value.slice(0, 30))
</script>

<style scoped lang="sass">
.photo-table
  max-width: 600px
  max-height: 600px
  margin: 0 auto
  overflow: auto

  table
    width: 100%
    border-collapse: collapse

    th, td
      padding: 8px
      border: 1px solid #ccc
      text-align: left
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis

    th
      cursor: pointer
      background-color: #f4f4f4
</style>