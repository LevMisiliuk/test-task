<template>
  <transition name="fade">
    <div v-if="isVisible" class="modal">
      <div class="modal-content">
        <h1 class="modal__header">{{ movie.title }}</h1>
        <div class="modal__header__subtitle">
          <p>
            <span>Genre:</span>
            {{ movie.genre_type }}
          </p>
          <p>
            <span>IMDB:</span>
            {{ movie.rating_score }}/10
          </p>
          <p>
            <span>Year:</span>
            {{ movie.year }}
          </p>
        </div>
        <img @click="closeModal" class="close" src="@/assets/images/cross.svg" />
        <div class="video-wrapper">
          <video controls>
            <source :src="movie.trailer_link" type="video/mp4" />
            Your browser does not support the video tag.
          </video>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'ModalMovie',
  props: {
    movie: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      isVisible: false
    }
  },
  methods: {
    toggleModal() {
      this.isVisible = !this.isVisible
    },
    openModal() {
      this.isVisible = true
    },
    closeModal() {
      this.isVisible = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/mixins.scss';
.modal {
  &__header {
    text-decoration: underline;
    &__subtitle {
      display: flex;
      gap: 20px;
      margin-bottom: 10px;
    }
  }
  backdrop-filter: blur(5px);
  color: #000;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 999;
  transition: opacity 0.5s ease-in-out;

  @include respond-to('laptop') {
    width: 100%;
    flex-direction: column;
    align-items: center;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.modal-content {
  position: relative;
  background-color:	#f5f5f5;
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 16px;
  width: 840px;
  height: 460px;
  transition: transform 0.3s ease-in-out;

  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1), 0px 1px 3px rgba(0, 0, 0, 0.1);

  @include respond-to('tablet') {
    width: 90%;
    height: auto;
    padding: 10px;
  }
}

.modal__header {
  margin-bottom: 5px;
  text-decoration: underline;
  color: #333;
}

.close {
  width: 20px;
  height: 20px;
  cursor: pointer;
  position: absolute;
  top: 10px;
  right: 10px;
  filter: brightness(0.7);
}

.video-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 800px;
  height: 420px;
  overflow: hidden;

  @include respond-to('tablet') {
    width: 100%;
    height: auto;
  }
}

video {
  width: 700px;
  height: 320px;

  @include respond-to('tablet') {
    width: 100%;
    height: auto;
  }
}
.loader-wrapper {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
}
</style>
