<template>
  <div v-if="isObjectRandomMovie" class="movie">
    <div class="movie__inner">
      <img
        :class="['movie__img', { blurry: isBlurred }]"
        :src="randomMovie.image_path"
        :alt="randomMovie.title"
      />
      <div class="movie__info">
        <h1 class="movie__title">
          {{ randomMovie.title }}
        </h1>
        <div class="movie__scores">
          <p class="movie__scores_text">
            <span class="bold">Genre:</span>
            {{ randomMovie.genre_type }}
          </p>
          <p class="movie__scores_text">
            <span class="bold">IMDB:</span>
            {{ randomMovie.rating_score }}/10
          </p>
          <p class="movie__scores_text">
            <span class="bold">Year:</span>
            {{ randomMovie.year }}
          </p>
        </div>
        <div class="movie__description">
          <p class="movie__description__text">{{ randomMovie.description }}</p>
          <button @click="openDialog" class="primary-button u-margin-bottom">Watch</button>
        </div>
      </div>
    </div>
  </div>
  <div v-else-if="isStringRandomMovie" class="status-message">
    {{ randomMovie }}
  </div>
  <ModalMovie :movie="randomMovie" ref="modal" />
  <BasicLoader class="loader-position" v-if="isLoading" />
</template>

<script>
import BasicLoader from '../loader/BasicLoader.vue'
import ModalMovie from './modal/ModalMovie.vue'

export default {
  name: 'MovieList',
  components: {
    BasicLoader,
    ModalMovie
  },
  props: {
    randomMovie: {
      type: [Object, String],
      default: () => {}
    }
  },
  data() {
    return {
      isLoading: false,
      isBlurred: false
    }
  },
  computed: {
    isObjectRandomMovie() {
      return !this.isLoading && typeof this.randomMovie === 'object'
    },
    isStringRandomMovie() {
      return !this.isLoading && typeof this.randomMovie === 'string'
    }
  },
  watch: {
    randomMovie: {
      handler() {
        this.setLoadingAndBlur()
        setTimeout(() => {
          this.removeLoading()
          setTimeout(() => {
            this.removeBlur()
          }, 200)
        }, 1000)
      },
      immediate: true
    }
  },
  methods: {
    openDialog() {
      this.$refs.modal.openModal()
    },
    setLoadingAndBlur() {
      this.isBlurred = true
      this.isLoading = true
    },
    removeLoading () {
      this.isLoading = false
    },
    removeBlur () {
      this.isBlurred = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/mixins.scss';
.movie {
  &__img {
    margin-right: 20px;
    transition: filter 1s ease;
    @include respond-to('tablet') {
      margin-right: 0;
    }
  }
  &__img.blurry {
    filter: blur(20px);
  }

  &__img:not(.blurry) {
    filter: blur(0);
  }

  &__info {
    max-width: 350px;
  }

  &__title {
    font-weight: bold;
    text-decoration: underline;
  }

  &__inner {
    display: flex;
    @include respond-to('laptop') {
      display: flex;
      justify-content: center;
      margin: 20px 10px 0 10px;
    }
    @include respond-to('tablet') {
      display: flex;
      flex-direction: column;
    }
  }

  &__scores {
    display: flex;
    margin-bottom: 20px;
    &_text {
      margin: 0 20px 0 0;
    }
  }

  &__description {
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    margin: 20px 0 20px 0;

    &__text {
      font-size: 16px;
      margin-bottom: 20px;
    }
  }
}
.loader-position {
  height: 20vh;
  margin: auto;
}
.status-message {
  font-size: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 20vh;
  margin: auto;
}
.bold {
  font-weight: bold;
}
</style>
