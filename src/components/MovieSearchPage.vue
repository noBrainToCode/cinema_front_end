<template>
    <movie-header-backup></movie-header-backup>
    <div>
        <div class="grid">
            <div class="card" v-for="movie in movies.arr" @click="goToMovieDetail(movie.movieId)">
                <img :src="getPosterUrl(movie.posterName)" alt="Movie Poster" />
                <div class="info"><h3>{{ movie.movieName }}</h3></div>
                <div class="info">
                <span><b>评分：</b>{{ movie.rate }}</span>
                <span><b>领域：</b>{{ movie.genre }}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    import axios from 'axios';
    import MovieHeaderBackup from './MovieHeaderBackup.vue';
    import { reactive } from 'vue';
    import router from '..';
    
    const movies = reactive({
        arr: []
    });

    async function fetchMovies() {
        try {
            const partText = router.currentRoute.value.params.partText;
            // const response = await axios.get(`/searchMovies/${partText}`);
            const response = await axios.get(`${config.url}movies/1`);
            if (response.data.code == 200) {
                movies.arr = response.data.data.list;
            }
        } catch (error) {
            console.log(error);
        }
    }

    function goToMovieDetail(movieId) {
        router.push(`/movie/${movieId}`)
    }

    function getPosterUrl(posterName) {
        return require(`@/assets/img/movie_posts/${posterName}`);
    }

    fetchMovies();
</script>