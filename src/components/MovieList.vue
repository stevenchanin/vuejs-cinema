<template>
  <div id="movie-list">
    <div v-if="filteredMovies.length">
      <movie-item
        v-for="movie in filteredMovies"
        v-bind:key="movie.id"
        v-bind:movie="movie.movie">

        <div class="movie-sessions">
          <div v-for="session in filteredSessions(movie.sessions)" class="session-time-wrapper">
            <div class="session-time">{{ formatSessionTime(session.time) }}</div>
          </div>
        </div>

      </movie-item>
    </div>
    <div v-else-if="movies.length" class="no-results">
      {{ noResults }}
    </div>
    <div v-else class="no-results">
      Loading...
    </div>
  </div>
</template>
<script>
  import genres from '../util/genres.js';
  import times from '../util/times.js';
  import MovieItem from './MovieItem.vue';

  export default {
    props: [ 'genre', 'time', 'movies', 'day' ],

    methods: {
      moviePassesGenreFilter: function(movie) {
        if (this.genre.length) {
          let movieGenres = movie.movie.Genre.split(", ");
          let matched = true;

          this.genre.forEach(genre => {
            if (movieGenres.indexOf(genre) === -1) {
              matched = false;
            }
          });
          return matched;
        } else {
          return true;
        }
      },

      sessionPassesTimeFilter(session) {
        if (!this.day.isSame(this.$moment(session.time), 'day')) {
          return false;
        } else if (this.time.length === 0 || this.time.length == 2) {
          return true;
        } else if (this.time[0] === times.AFTER_6PM ) {
          return this.$moment(session.time).hour() >= 18;
        } else {
          return this.$moment(session.time).hour() < 18;
        }
       },

      formatSessionTime(raw) {
        return this.$moment(raw).format('h:mm A');
      },

      filteredSessions(sessions) {
        return sessions.filter(this.sessionPassesTimeFilter);
      }
    },

    computed: {
      filteredMovies() {
        return this.movies
                .filter(this.moviePassesGenreFilter)
                .filter(movie => movie.sessions.find(this.sessionPassesTimeFilter));
      },

      noResults() {
        var appliedFilters = this.time.concat(this.genre).join(', ');
        return `No results for ${appliedFilters}.`;
      }
    },

    components: { MovieItem }
  }
</script>
