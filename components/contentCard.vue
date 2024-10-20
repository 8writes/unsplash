<!-- @format -->

<template>
  <div class="content-card" :style="{ backgroundImage: `url(${image})` }">
    <div class="content-card__wrapper">
        <h2 class="content-card__title">{{ author }}</h2>
        <p class="content-card__description">{{ location }}</p>
    </div>
    <div v-if="isModalOpen" class="modal" @click="closeModal">
      <div class="modal-content" @click.stop>
        <img :src="image" alt="" class="modal-image" />
        <h2 class="modal-author">{{ author }}</h2>
        <p class="modal-location">{{ location }}</p>
        <button class="close-modal" @click="closeModal">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    image: {
      type: String,
      required: true,
    },
    author: {
      type: String,
      required: true,
    },
    location: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isModalOpen: false, // Modal visibility state
    };
  },
  methods: {
    openModal() {
      this.isModalOpen = true; // Open modal
    },
    closeModal() {
      this.isModalOpen = false; // Close modal
    },
  },
}
</script>

<style lang="scss">
@import '../assets/scss/contentCard.scss';
</style>
