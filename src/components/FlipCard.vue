<template>
  <div class="flip-card" @click="flipCard">
    <div class="card-container" :class="{ flipped: isFlipped }">
      <div class="front">
        <img :src="image" class="card-image" />
      </div>
      <div class="back">
        <div class="card-header">{{ header }}</div>
        <div class="card-text-scroll">
          <p>{{ cardText }}</p>
        </div>
        <!-- New Lyrics Button -->
        <button
          v-if="lyricsAvailable"
          class="lyrics-button"
          @click.stop="showLyrics"
        >
          View Lyrics
        </button>
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
    isFlipped: { type: Boolean, default: false },
    lyricsAvailable: { type: Boolean, default: false },
    lyricsId: { type: Number, default: null },
  },
  data() {
    return {
      cardText: this.textPath,
    };
  },
  methods: {
    flipCard() {
      console.log("FlipCard: Flip method called");
      console.log("Current isFlipped state:", this.isFlipped);
      // Emit the flip event to the parent
      this.$emit("flip");
    },
    showLyrics() {
      console.log("FlipCard: Show lyrics button clicked");
      if (this.lyricsAvailable) {
        this.$emit("show-lyrics", this.lyricsId);
      }
    },
    watch: {
      isFlipped(newValue) {
        console.log("FlipCard: isFlipped changed to", newValue);
      },
    },
  },
};
</script>

<style scoped>
.flip-card {
  flex: 0 0 calc(33.333% - 14px);
  max-width: calc(33.333% - 14px);
  scroll-snap-align: start;
  min-width: 250px;
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
  width: calc(100% - 2px);
  height: calc(100% - 2px);
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
  object-fit: contain;
  display: block;
  margin: auto;
}
.card-header {
  width: 100%;
  padding: 10px;
  text-align: center;
  flex-shrink: 0;
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
.card-text-scroll {
  padding: 10px;
  overflow-y: auto;
  height: calc(100% - 60px);
}

.lyrics-button {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.lyrics-button:hover {
  background-color: #0056b3;
}
</style>
