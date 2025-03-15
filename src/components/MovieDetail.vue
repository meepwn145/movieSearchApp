<template>
  <div v-if="movie" class="movie-container">
    <img :src="movie.Poster" alt="Movie Poster" class="poster" />
    <div class="movie-details">
    <img 
        src="/play.jpg" 
        alt="Play" 
        class="play-button"
      />
      <h2>{{ movie.Title }} ({{ movie.Year }})</h2>
      
      <p><strong>Director:</strong> {{ movie.Director }}</p>
      <p><strong>Actors:</strong> {{ movie.Actors }}</p>
      <p><strong>Plot:</strong> {{ movie.Plot }}</p>
      <p><strong>Rated:</strong> {{ movie.Rated }}</p>
      <p><strong>Genre:</strong> {{ movie.Genre }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['movie']
};
</script>

<style scoped>
.movie-container {
  display: flex;
  gap: 20px; 
  align-items: center; 
}

.poster {
  width: 300px;
  border-radius: 8px;
  object-fit: cover;
}

.movie-details {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.play-button {
  width: 100px;
  height: 100px;
  border-radius: 100%; 
  object-fit: cover; 
  border: 1px solid #ff0000; 
  cursor: pointer;
  transition: transform 0.2s ease;
}

.play-button:hover {
  transform: scale(1.1); 
}
</style>
