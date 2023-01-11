<template>
  <div @click="onClick" class="modaloverlay">
    <div class="rootcontainer">
    <div id="carousel-container" @click.stop class="carouselContainerOpen">
      <div
      id="main-image-viewer"
      class="w-full h-[22rem] max-h-[24rem] rounded-xl bg-white flex overflow-hidden relative"
    >
      <div
        class="absolute top-0 left-0 p-4 h-full flex items-center justify-center"
      >
        <button
          @click="prevSlide()"
          id="prev-btn"
          class="w-8 h-8 z-[97] shrink-0 rounded-full flex items-center justify-center bg-white/50 text-white"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-chevron-left"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z"
            />
          </svg>
        </button>
      </div>
      <div
        class="absolute top-0 right-0 p-4 h-full flex items-center justify-center"
      >
        <button
          @click="nextSlide()"
          id="next-btn"
          class="w-8 h-8 z-[97] shrink-0 rounded-full flex items-center justify-center bg-white/50 text-white"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-chevron-right"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"
            />
          </svg>
        </button>
      </div>
      <div
        id="image-slider-container"
        @click="toggleModal"
        class="w-full h-full flex cursor-pointer"
      >
        <div
          ref="imageSlider"
          id="image-slider"
          :style="carouselPos"
          class="imageSlider"
        >
          <div
            id="image-preview-card"
            v-for="item in assets"
            :key="item.id"
            class="imagePreviewCard"
          >
            <div
              id="video-container"
              class="w-full h-full relative cursor-pointer"
              v-if="item.file_type === 'video'"
            >
              <video :src="item.url" class="w-full h-full object-cover"></video>
              <button
                v-if="!isPlayed"
                class="z-[1] h-20 w-20 flex items-center justify-center text-white text-2xl bg-white/20 rounded-full absolute top-[50%] left-[50%] -translate-y-[50%] -translate-x-[50%]"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="35"
                  height="35"
                  fill="currentColor"
                  class="bi bi-play-circle"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
                  />
                  <path
                    d="M6.271 5.055a.5.5 0 0 1 .52.038l3.5 2.5a.5.5 0 0 1 0 .814l-3.5 2.5A.5.5 0 0 1 6 10.5v-5a.5.5 0 0 1 .271-.445z"
                  />
                </svg>
              </button>
            </div>
            <div
              v-if="item.file_type === 'youtube'"
              class="w-full h-full z-[4]"
            >
              <iframe :src="item.url" class="w-full h-full z-[4]"></iframe>
            </div>
            <div id="img-container" class="w-full h-full" v-else>
              <img :src="item.url" alt="" class="w-full h-full object-cover" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      id="preview-image-list"
      class="h-auto w-full flex items-center justify-between gap-2"
    >
      <div
        id="prev-btn-container"
        class="w-20 h-full flex items-center justify-center"
      >
        <button
          @click="prevPreviewSlide()"
          id="prev-btn"
          class="w-8 h-8 shrink-0 rounded-full flex items-center justify-center bg-blue-200 text-blue-500"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-chevron-left"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z"
            />
          </svg>
        </button>
      </div>
      <div id="preview-container" class="w-full h-auto flex overflow-hidden">
        <div
          id="preview-slide"
          ref="imagePreviewSlider"
          :style="carouselPreviewPos"
          class="imagePreviewSlider"
        >
          <div
            v-for="(item, index) in assets"
            :key="item.id"
            :class="[currentSlide === index && 'selected', 'preview-item']"
          >
            <div
              @click="handleImageClick(index)"
              class="w-full h-full"
              v-if="item.file_type === 'video'"
            >
              <video :src="item.url" class="w-full h-full object-cover"></video>
            </div>
            <div
              @click="handleImageClick(index)"
              v-if="item.file_type === 'youtube'"
              class="w-full h-full"
            >
              <img
                :src="item.thumbnailUrl"
                alt="image"
                class="w-full h-full object-cover"
              />
            </div>
            <div @click="handleImageClick(index)" class="w-full h-full" v-else>
              <img
                :src="item.url"
                alt="image"
                class="w-full h-full object-cover"
              />
            </div>
          </div>
        </div>
      </div>

      <div
        id="next-btn-container"
        class="w-20 h-full flex items-center justify-center"
      >
        <button
          @click="nextPreviewSlide()"
          id="next-btn"
          class="w-8 h-8 shrink-0 rounded-full flex items-center justify-center bg-blue-200 text-blue-500"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-chevron-right"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"
            />
          </svg>
        </button>
      </div>
    </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  props: {
    files: {
      type: Array,
      required: true,
    },
    isOpened: {
      type: Boolean,
      required: true,
    },
    currentIndex: {
      type: Number,
      required: true,
    },
    onClick: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      assets: [...this.files],
      isPlayed: false,
      carouselPos: {},
      carouselPreviewPos: {},
      sliderWidth: 0,
      sliderPreviewWidth: 0,
      carouselStep: 0,
      carouselPreviewStep: 0,
      currentSlide: 0,
      carouselMoveX: 0,
      carouselPreviewMoveX: 0,
      selected: "w-24 h-20 rounded-xl ring-4 ring-slate-700",
      notSelected: "w-24 h-20 rounded-xl ring-4 ring-slate-700",
    };
  },
  mounted() {
    this.currentSlide = this.currentIndex;
    this.setStep();
    this.setStepPreview();
    this.handleImageClick(this.currentSlide);
    console.log("mounted")
  },
  methods: {
    setStep() {
      const sliderWidth = this.$refs.imageSlider.scrollWidth;
      const totalImage = this.assets.length;
      this.sliderWidth = sliderWidth;
      this.carouselStep = sliderWidth / totalImage;
    },
    setStepPreview() {
      const sliderPreviewWidth = this.$refs.imagePreviewSlider.scrollWidth;
      const totalImage = this.assets.length;
      this.sliderPreviewWidth = sliderPreviewWidth;
      this.carouselPreviewStep = sliderPreviewWidth / totalImage;
    },
    handleImageClick(index) {
      this.currentSlide = index;
      this.carouselMoveX = this.carouselStep * index;
      this.carouselPos = {
        transform: `translateX(-${this.carouselMoveX}px)`,
      };
    },
    playPauseVideo() {
      const video = this.$refs.video[0];
      if (video.paused) {
        video.play();
        this.isPlayed = true;
        console.log("play");
      } else {
        video.pause();
        this.isPlayed = false;
        console.log("pause");
      }
    },
    nextSlide() {
      if (this.currentSlide === this.assets.length - 1) {
        this.currentSlide = 0;
      } else {
        this.currentSlide += 1;
      }
      this.addStep();
      this.carouselPos = {
        transform: `translateX(-${this.carouselMoveX}px)`,
      };
    },
    prevSlide() {
      if (this.currentSlide === 0) {
        this.currentSlide = this.assets.length - 1;
      } else {
        this.currentSlide -= 1;
      }
      this.substractStep();
      this.carouselPos = {
        transform: `translateX(-${this.carouselMoveX}px)`,
      };
    },
    addStep() {
      if (this.carouselMoveX >= this.sliderWidth - this.carouselStep) {
        this.carouselMoveX = 0;
      } else {
        this.carouselMoveX += this.carouselStep;
      }
    },
    substractStep() {
      if (this.carouselMoveX <= 0) {
        this.carouselMoveX = this.sliderWidth - this.carouselStep;
      } else {
        this.carouselMoveX -= this.carouselStep;
      }
    },
    nextPreviewSlide() {
      this.addPreviewStep();
      this.carouselPreviewPos = {
        transform: `translateX(-${this.carouselPreviewMoveX}px)`,
      };
      console.log(this.carouselPreviewMoveX);
    },
    prevPreviewSlide() {
      this.substractPreviewStep();
      this.carouselPreviewPos = {
        transform: `translateX(-${this.carouselPreviewMoveX}px)`,
      };
      console.log(this.carouselPreviewMoveX);
    },
    addPreviewStep() {
      if (
        this.carouselPreviewMoveX >=
        this.sliderPreviewWidth - this.carouselPreviewStep
      ) {
        this.carouselPreviewMoveX = 0;
      } else {
        this.carouselPreviewMoveX += this.carouselPreviewStep;
      }
    },
    substractPreviewStep() {
      if (this.carouselPreviewMoveX <= 0) {
        this.carouselPreviewMoveX = 0;
      } else {
        this.carouselPreviewMoveX -= this.carouselPreviewStep;
      }
    },
  },
};
</script>

<style scoped>
.imageSlider {
  z-index: 1;
  display: flex;
  transition: transform 0.3s ease;
}

.imagePreviewSlider {
  z-index: 1;
  display: flex;
  width: 100%;
  gap: 0.5rem;
  transition: transform 0.3s ease;
}

.imagePreviewCard {
  width: 100%;
  height: 100%;
  flex-shrink: 0;
}

.preview-item {
  height: auto;
  border-radius: 15px;
  overflow: hidden;
  flex: 0 0 calc(20% - 0.5rem);
}

.selected {
  border: 4px solid #1e40af;
}

@media (max-width: 784px) {
  .preview-item {
    flex: 0 0 calc(33.33% - 0.5rem);
  }
}

.modaloverlay {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 99;
  overflow: hidden;
}

.rootcontainer {
  position: relative;
  display: flex;
  flex-direction: column;
  padding: 2rem;
  width: 100%;
  height: 100%;
  align-items: center;
  justify-content: center;
}

.carouselContainerOpen {
  width: 100%;
  max-width: 800px;
  background-color: #fff;
  border-radius: 15px;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  overflow: hidden;
  scale: 1.1;
  z-index: 100;
}
</style>
