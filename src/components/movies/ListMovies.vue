<template>
    <div class="container-movies d-middle p-100">

       <CardMovie 
            v-for="movie in store.listMovies"
            v-if="store.listMovies"
            :id="movie.imdbID"
            :poster="movie.Poster"
            :title="movie.Title"
            :year="movie.Year"
            :type="movie.Type"
       />
       <div class="d-middle w-100" v-else>
            <p>No hay resultados en la busqueda</p>
       </div>

    </div>

    <div v-for="movie in movies" :key="movie.id" class="movie-item">
        <h2>{{ movie.title }}</h2>
        <p>{{ movie.description }}</p>
        <router-link :to="`/movies/${movie.id}`" class="details-link">Ver Detalles</router-link>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import axios from 'axios'

import CardMovie from "@/components/movies/CardMovie.vue"
import { useMoviesStore } from "@/store/movies.store"

const store = useMoviesStore()
onMounted(()=> {
    console.log('ListMovies mounted')
    fetchMovies()
})

async function  fetchMovies() {
  await store.getMovies()
}  

export default {
  setup() {
    const movies = ref([]);

    onMounted(async () => {
      try {
        const response = await axios.get('/api/movies');
        movies.value = response.data;
      } catch (error) {
        console.error("Error al cargar las películas:", error);
      }
    });

    return { movies };
  }
}
</script>

<style scoped lang="scss">
.container-movies {
    flex-wrap: wrap;
    gap: 20px
}

p{
    color: $white;
}

.movie-item {
  margin-bottom: 20px;
}
.details-link {
  color: blue;
  text-decoration: underline;
}
</style>    