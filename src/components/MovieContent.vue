<template>
  <movie-header-backup></movie-header-backup>
  <div class="search-bar">
    <input type="text" placeholder="Search" @keydown="SearchButtonDown" v-model="search"/>
  </div>
  <div>
    <div class="grid">
      <div class="card" v-for="(movie, index) in movies" :key="index" @click="goToMovieDetail(movie.movieId)">
        <img :src="getPosterUrl(movie.posterName)" alt="Movie Poster" class="movie-poster"/>
        <div class="info"><h3>{{ movie.movieName }}</h3></div>
        <div class="info">
          <span><b>评分：</b>{{ movie.rate }}</span>
          <span><b>领域：</b>{{ movie.genre }}</span>
        </div>
      </div>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
  <movie-footer></movie-footer>
</template>

<script>
import axios from 'axios';
import MovieFooter from './MovieFooter.vue';
import MovieHeaderBackup from './MovieHeaderBackup.vue';
import config from '@/config';

export default {
  components: {
    MovieFooter,
    MovieHeaderBackup
  },
  data() {
    return {
      movies: [],
      currentPage: 1,
      totalPages: 5,
      search: ''
    };
  },
  created() {
    this.fetchMovies();
  },
  methods: {
    async fetchMovies() {
      try {
        const response = await axios.get(`${config.url}movies/${this.currentPage}`);
        if (response.data.code === 200) {
          this.movies = response.data.data.list;
          this.totalPages = response.data.data.pages;
        } else {
          alert('Failed to fetch movies');
        }
      } catch (error) {
        console.error('Error fetching movies:', error);
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.fetchMovies();
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.fetchMovies();
      }
    },
    goToMovieDetail(movieId) {
      this.$router.push(`/movie/${movieId}`);
    },
    getPosterUrl(posterName) {
      return require(`@/assets/img/movie_posts/${posterName}`);
    },
    SearchButtonDown(event) {
      if (event.code == "Enter") {
        // 如果没有输入，就退回主页
        if (this.search.length == 0) {
          this.$router.push("/");
        } else {
          this.$router.push(`/searchMovies/${this.search}`);
        }
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.pagination {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.pagination button {
  margin: 0 10px;
  padding: 10px 20px;
}

.movie-poster {
  height: 444.44px;
}

</style>
