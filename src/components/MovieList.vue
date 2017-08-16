<template>
  <div id="movie-list">
    <div
      v-for="movie in filteredMovies"
      class="movie"
      v-bind:key="movie.id">
      {{ movie.title }}
    </div>
  </div>
</template>
<script>
  import genres from '../util/genres.js';

  export default {
    props: [ 'genre', 'time' ],

    data() {
      return {
        movies: [
          { id: 1, title: 'Pulp Fiction', genre: genres.CRIME },
          { id: 2, title: 'Home Alone', genre: genres.COMEDY },
          { id: 3, title: 'Austin Powers', genre: genres.COMEDY }
        ]
      }
    },

    methods: {
      moviePassesGenreFilter: function(movie) {
        if (this.genre.length) {
          return this.genre.find(genre => movie.genre === genre)
        } else {
          return true;
        }
      }
    },

    computed: {
      filteredMovies() {
        return this.movies.filter(this.moviePassesGenreFilter)
      }
    }
  }
</script>
