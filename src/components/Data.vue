<template>
  <div class="top">
    <input type="text" v-model="searchQuery" placeholder="Search by title" />
    <h1>Photos</h1>

    <div v-if="loading">Loading...</div>
    <div v-else>
      <div class="container">
        <div v-for="photo in filteredPhotos" :key="photo.id" class="cont">
          <img :src="photo.url" :alt="photo.title" />
          <p>{{ photo.title }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, computed } from "vue";
import axios from "axios";

export default {
  name: "Data",
  setup() {
    const photos = ref([]);
    const loading = ref(true);
    const searchQuery = ref("");

    onMounted(async () => {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/photos"
        );
        photos.value = response.data;
        loading.value = false;
      } catch (error) {
        console.error("Error fetching photos:", error);
        loading.value = false;
      }
    });

    const filteredPhotos = computed(() => {
      return photos.value.filter((photo) =>
        photo.title.toLowerCase().startsWith(searchQuery.value.toLowerCase())
      );
    });

    return {
      photos,
      loading,
      searchQuery,
      filteredPhotos,
    };
  },
};
</script>

<style scoped>
.cont {
  display: flex;
  flex-direction: column;
}

.cont img {
  height: 25rem;
  width: 25rem;
}

.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}

.top {
  margin: 3rem;
}

.top input {
  width: 15rem;
  height: 2rem;
  border: 2px solid black;
  border-radius: 10px;
}
</style>
