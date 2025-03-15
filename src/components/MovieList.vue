<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="movies.length">
      <div 
        v-for="movie in movies" 
        :key="movie.imdbID" 
        @click="$emit('select', movie.imdbID)"
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
img {
  width: 100px;
  height: 150px;
}
</style>
