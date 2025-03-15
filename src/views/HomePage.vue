<template>
  <div class="min-h-screen flex justify-center bg-white-900">
    <!-- White Centered Content -->
    <div class="w-full max-w-5xl bg-white text-white flex flex-col min-h-screen">
      <!-- Header -->
      <header class="fixed-header">
         <h1 @click="resetApp" class="movie-app-title">
          🎬 Movie Recommendations
         </h1>

        <div class="search-bar-wrapper">
           <MovieSearch @search="fetchMovies" />
        </div>
      </header>

        <br />
        <br />
        <br />
        <br />
        <br />
        <div class="relative w-full max-w-2xl mx-auto flex flex-col items-center justify-center min-h-[70vh]">
                <!-- Recommendations -->
                <div v-if="!hasSearched && recommendations.length" class="w-full mt-6">
        <h2 class="recommendation-title">
          🍿 Here's some recommendations of movies:
        </h2>

        <div class="recommendation-grid">
            <div 
              v-for="(movie, index) in recommendations" 
              :key="movie.imdbID"
              @click="fetchMovieDetails(movie.imdbID)"
              class="cursor-pointer bg-gray-200 p-4 rounded-lg border border-gray-300 hover:border-blue-400 transition relative"
            >
              <img 
                :src="movie.Poster !== 'N/A' ? movie.Poster : 'https://via.placeholder.com/150'" 
                alt="Movie Poster" 
                class="w-full h-48 object-cover rounded-md mb-2"
              />
              <h3 class="text-lg font-semibold text-gray-900">{{ movie.Title }}</h3>
              <p class="text-gray-600 text-sm">{{ movie.Year }}</p>
            
            </div>
          </div>
        </div>

        <MovieList v-if="movies.length || hasSearched" :movies="movies" :loading="loading" @select="fetchMovieDetails" />
        <p v-if="!movies.length && hasSearched" class="no-results">
          No results found.
        </p>

        <MovieDetail v-if="selectedMovie" :movie="selectedMovie" class="mt-6 border border-gray-300 p-4 rounded-lg bg-gray-200" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import MovieSearch from '../components/MovieSearch.vue';
import MovieList from '../components/MovieList.vue';
import MovieDetail from '../components/MovieDetail.vue';

export default {
  components: { MovieSearch, MovieList, MovieDetail },
  data() {
    return {
      movies: [],
      recommendations: [],
      selectedMovie: null,
      loading: false,
      hasSearched: false
    };
  },
  methods: {
    async fetchMovies(query) {
      this.loading = true;
      this.selectedMovie = null;
      this.hasSearched = true;
      try {
        const res = await axios.get(
          `https://www.omdbapi.com/?apikey=${import.meta.env.VITE_OMDB_API_KEY}&s=${query}`
        );
        if (res.data.Response === 'True') {
          this.movies = res.data.Search;
        } else {
          this.movies = [];
          console.error('No results found:', res.data.Error);
        }
      } catch (error) {
        console.error('Error fetching movies:', error);
      } finally {
        this.loading = false;
      }
    },

    async fetchMovieDetails(imdbID) {
      try {
        const res = await axios.get(
          `https://www.omdbapi.com/?apikey=${import.meta.env.VITE_OMDB_API_KEY}&i=${imdbID}`
        );
        this.selectedMovie = res.data;
      } catch (error) {
        console.error('Error fetching movie details:', error);
      }
    },

    async fetchRecommendations() {
      const sampleTitles = [
        'Inception',
        'The Matrix',
        'Interstellar',
        'The Dark Knight',
        'Pulp Fiction',
        'Fight Club',
        'Forrest Gump'
      ];

      const shuffledTitles = sampleTitles.sort(() => 0.5 - Math.random()).slice(0, 3);

      try {
        this.recommendations = [];
        for (const title of shuffledTitles) {
          const res = await axios.get(
            `https://www.omdbapi.com/?apikey=${import.meta.env.VITE_OMDB_API_KEY}&t=${title}`
          );
          if (res.data.Response === 'True') {
            this.recommendations.push(res.data);
          }
        }
      } catch (error) {
        console.error('Error fetching recommendations:', error);
      }
    },

    resetApp() {
      this.movies = [];
      this.selectedMovie = null;
      this.hasSearched = false;
      this.fetchRecommendations(); // Reload recommendations
    }
  },
  mounted() {
    this.fetchRecommendations();
  }
};
</script>

<style scoped>
/* Fixed header style */
.fixed-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 70px; 
  background-color: rgba(0, 0, 0, 0.8); 
  color: white;
  padding: 10px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  z-index: 50;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

/* Center search bar */
.search-bar-wrapper {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
/* Title style */
.movie-app-title {
  background-color: rgba(120, 40, 31, 0.7); 
  color: white;
  font-weight: bold;
  padding: 10px 20px;
  display: inline-block;
  border-radius: 8px;
  cursor: pointer;
}
.recommendation-title {
  font-size: 36px; /* Adjust size as needed */
  font-weight: 700; /* Bold */
  color: #eaeded; /* Dark gray for better visibility */
  text-align: center;
  margin-bottom: 16px;
}

.no-results {
  font-size: 36px;
  font-weight: bold;
  color: white;
  margin-top: 1rem;
  text-align: center;
}

.recommendation-grid {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.movie-card {
  background-color: #f3f3f3;
  padding: 16px;
  border-radius: 8px;
  border: 1px solid #ccc;
  width: 320px;
  text-align: center;
  transition: box-shadow 0.3s ease;
}

.movie-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.movie-poster {
  width: 100%;
  height: 400px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 12px;
}

.movie-title {
  font-size: 18px;
  font-weight: bold;
  color: #2d2d2d;
}

.movie-year {
  font-size: 14px;
  color: #555;
}

/* Content wrapper padding to prevent overlap */
.content-wrapper {
  padding-top: 80px; 
}
</style>