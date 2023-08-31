<template>
  <div class="container">
    <div class="filters">
      <div class="filters__box">
        <FiltersBox
          :geners="allGenres"
          :rating="allRating"
          :years="allYears"
          @selection="updateSelectedValues"
        />
        <button @click="getRandomMovie" class="primary-button-large">
          <BasicLoader v-if="isLoading" />
          <span v-if="!isLoading">Get Movie</span>
        </button>
      </div>
      <MovieData v-if="randomMovie" :random-movie="randomMovie" />
    </div>
  </div>
</template>

<script>
import MovieData from '../movie-data/MovieData.vue'
import moviesData from '../../data/movies_list.json'
import FiltersBox from '../filters/FiltersBox.vue'
import BasicLoader from '../loader/BasicLoader.vue'

export default {
  name: 'MainScreen',
  components: {
    FiltersBox,
    MovieData,
    BasicLoader
  },
  data() {
    return {
      movies: moviesData,
      selectedFilters: null,
      randomMovie: null,
      isLoading: false,
      statusMessage: false
    }
  },
  computed: {
    allGenres() {
      const genresSet = new Set(this.movies.map((item) => item.genre_type))
      return Array.from(genresSet).map((genre, index) => {
        return {
          value: genre,
          id: index,
          label: genre
        }
      })
    },
    allRating() {
      const ratingsSet = new Set(this.movies.map((item) => item.rating_score))
      return Array.from(ratingsSet).map((rating, index) => {
        return {
          value: rating,
          id: index,
          label: String(rating)
        }
      })
    },
    allYears() {
      const yearsSet = new Set(this.movies.map((item) => item.year))
      const sortedYears = Array.from(yearsSet).sort((a, b) => b - a)
      return sortedYears.map((year, index) => {
        return {
          value: year,
          id: index,
          label: String(year)
        }
      })
    }
  },
  methods: {
    getRandomMovie() {
      // Resetting the randomMovie and showing loader
      this.randomMovie = null
      this.isLoading = true

      this.$nextTick(() => {
        let moviesToChooseFrom = this.movies

        // Filtering the movies based on selected filters
        if (this.selectedFilters) {
          moviesToChooseFrom = this.movies.filter((movie) => {
            return Object.keys(this.selectedFilters).every((key) => {
              return (
                this.selectedFilters[key] === null ||
                String(movie[key]) === this.selectedFilters[key]
              )
            })
          })
        }

        // If there's no movie that matches these filters.
        if (!moviesToChooseFrom.length) {
          this.randomMovie = 'There\'s no movie that matches these filters 😓'
          this.isLoading = false
          return
        }

        // Random movie
        this.randomMovie = moviesToChooseFrom[Math.floor(Math.random() * moviesToChooseFrom.length)]

        // Actually we don't need it, just to show loader
        setTimeout(() => {
          this.isLoading = false
        }, 1000)
      })
    },
    updateSelectedValues(selectedValues) {
      this.selectedFilters = selectedValues
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/mixins.scss';
.filters {
  display: flex;
  justify-content: space-between;

  &__box {
    margin-right: 20px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start;
  }

  @include respond-to('laptop') {
    flex-direction: column;

    &__box {
      margin-right: 0;
    }
  }
}
</style>
