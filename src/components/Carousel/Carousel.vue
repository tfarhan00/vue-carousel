<template>
  <ImageCarousel :isOpened="isOpened" :toggleModal="toggleModal" />
  <Transition>
    <div
      v-show="isOpened"
      @click="toggleModal"
      class="z-[98] w-screen h-screen bg-black/40 fixed top-0 left-0 flex items-center justify-center"
    >
      <ImageCarousel />
    </div>
  </Transition>
</template>

<script>
import ImageCarousel from "./ImageCarousel.vue";
export default {
  components: {
    ImageCarousel,
  },
  data() {
    return {
      isOpened: false,
    };
  },
  methods: {
    toggleModal(e, isOpen) {
      e.stopPropagation();
      e.preventDefault();
      this.isOpened = !this.isOpened;
    },
  },
};
</script>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.3s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
