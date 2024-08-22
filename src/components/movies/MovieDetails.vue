<template>
  <div v-if="movie">
    <h1>{{ movie.title }}</h1>
    <p>{{ movie.description }}</p>
    <p><strong>Director:</strong> {{ movie.director }}</p>
    <p><strong>Release Date:</strong> {{ movie.releaseDate }}</p>
   
    <router-link to="/">Volver a la lista de películas</router-link>
  </div>
  <div v-else>
    <p>Cargando detalles de la película...</p>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

export default {
  setup() {
    const route = useRoute();
    const movie = ref(null);

    onMounted(async () => {
      try {
        const response = await axios.get(`/api/movies/${route.params.id}`);
        movie.value = response.data;
      } catch (error) {
        console.error("Error al cargar los detalles de la película:", error);
      }
    });

    return { movie };
  }
};
</script>

<style scoped>

</style>
