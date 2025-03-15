<template>
  <div class="search-component">
    <input type="text" v-model="query" placeholder="Search for movies..." />
    <button @click="searchMovies">Search</button>
  </div>
</template>


<script>
export default {
  data() {
    return {
      query: ''
    };
  },
  methods: {
    searchMovies() {
      if (this.query.trim()) {
        this.$emit('search', this.query);
      }
    }
  }
};
</script>

<style scoped>
.search-component button {
  font-weight: bold; 
  font-size: 18px;
  padding: 8px 12px;
  margin-left: 8px;
  background-color: #943126;
  color: #ffffff !important;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}


input {
  padding: 8px;
  width: 450px;
  font-size: 20px; 
  border: 1px solid #ccc;
  border-radius: 10px;
}
input::placeholder {
  font-weight: bold; 
  font-size: 16px;
  color: #666; 
}

button {
  font-weight: bold; 
  font-size: 18px;
  padding: 8px 12px;
  margin-left: 8px;
  background-color: #943126 ;
  color: white !important;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

button:hover {
  background-color: #45a049;
  color: white !important; 
}


</style>