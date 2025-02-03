<template>
  <div class="flip-card" @click="flipCard">
    <div class="card-container" :class="{ 'flipped': isFlipped }">
      <div class="front">
        <img :src="image" class="card-image">
      </div>
      <div class="back">
        <div class="card-header">{{ header }}</div>
        <div class="card-text-scroll">
          <p>{{ cardText }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    header: { type: String, required: true },
    image: { type: String, required: true },
    textPath: { type: String, required: true },
    isFlipped: { type: Boolean, default: false }
  },
  data() {
    return {
      isFlipped: false,
      cardText: this.textPath
    };
  },
  methods: {
    flipCard() {
      this.$emit('flip');
      this.isFlipped = !this.isFlipped;
    }
  }
};
</script>

<style scoped>
.flip-card {
    flex: 0 0 calc(33.333% - 14px); /* Adjusted to account for gap */
    max-width: calc(33.333% - 14px);
    scroll-snap-align: start; /* Added for better scrolling behavior */
    min-width: 250px; /* Added minimum width */
}

.card-container {
  width: 100%;
  height: 100%;
  aspect-ratio: 1 / 1;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  position: relative;
}

.card-container.flipped {
  transform: rotateY(180deg);
}

.front,
.back {
  position: absolute;
  width: calc(100% - 2px); /* Subtract 2px to account for 1px border on each side */
  height: calc(100% - 2px); /* Subtract 2px to account for 1px border on each side */
  backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: black 1px solid;
  border-radius: 10px;
}

.card-image {
  width: 100%;
  height: calc(100% - 40px);
  object-fit: contain; /* This preserves aspect ratio */
  display: block; /* Removes any extra space below image */
  margin: auto; /* Additional centering */
}

.card-header {
  width: 100%; /* Ensure header takes full width */
  padding: 10px;
  text-align: center;
  flex-shrink: 0; /* Prevents header from shrinking */
}

.front {
  z-index: 2;
  transform: rotateY(0deg);
}

.back {
  z-index: 1;
  transform: rotateY(180deg);
  overflow: hidden;

}

.card-header {
  padding: 10px;
  text-align: center;
}

.card-image {
  width: 100%;
  height: calc(100% - 40px);
  object-fit: contain;
}

.card-text-scroll {
  padding: 10px;
  overflow-y: auto;
  height: calc(100% - 60px);
}
</style>