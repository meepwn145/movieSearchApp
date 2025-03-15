<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="movies.length" class="movie-container">
      <div 
        v-for="movie in movies" 
        :key="movie.imdbID" 
        @click="$emit('select', movie.imdbID)"
        class="movie-item"
      >
        <img :src="movie.Poster" alt="Movie Poster" />
        <p>{{ movie.Title }} ({{ movie.Year }} )</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['movies', 'loading'],
};
</script>

<style scoped>
.movie-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center; /* Center horizontally */
  gap: 16px; /* Add space between posters */
  margin-top: 20px;
}

.movie-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  cursor: pointer; 
}

img {
  width: 100px;
  height: 150px;
}
</style>
