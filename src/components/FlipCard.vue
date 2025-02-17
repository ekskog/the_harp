<template>
  <div class="flip-card" @click="flipCard">
    <div class="card-container" :class="{ flipped: isFlipped }">
      <div class="front">
        <img :src="image" class="card-image" />
      </div>
      <div class="back">
        <div class="card-header">{{ header }}</div>
        <div class="card-id">Card ID: {{ cardNumber }}</div>
        <div class="card-text-scroll">
          <p>{{ cardText }}</p>
        </div>
        
        <button class="show-accordeon-button" @click.stop="showAccordeon">
          Show Album
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import AlbumAccordeon from "./AlbumAccordeon.vue";

export default {
  name: "FlipCard",
  components: {
    AlbumAccordeon,
  },
  props: {
    header: { type: String, required: true },
    image: { type: String, required: true },
    textPath: { type: String, required: true },
    isFlipped: { type: Boolean, default: false },
    cardNumber: { type: Number, required: true }, // <-- New prop
    accordeonId: { type: Number, required: true }, // <-- New prop
  },

  data() {
    return {
      cardText: this.textPath,
      AccordeonVisible: false,
    };
  },
  methods: {
    flipCard() {
      console.log("FlipCard: Flip method called");
      console.log("Current isFlipped state:", this.isFlipped);
      this.$emit("flip");
    },
    showAccordeon() {
      console.log("FlipCard: Accordeon button clicked: ", this.accordeonId);
      this.$emit("showAccordeon", this.accordeonId);
    },
  },
  watch: {
    isFlipped(newValue) {
      console.log("FlipCard: isFlipped changed to", newValue);
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
  border: 1px solid black;
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
  position: relative;
}
.card-text-scroll {
  padding: 10px;
  overflow-y: auto;
  height: calc(100% - 60px);
}
.accordeon-button {
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
.accordeon-button:hover {
  background-color: #0056b3;
}
.Accordeon-wrapper {
  margin-top: 10px;
  width: 100%;
}
</style>
