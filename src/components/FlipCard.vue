<template>
  <div class="flip-card" @click="flipCard">
    <div :class="{ 'flipped': isFlipped }">
      <div class="front">
        <div class="card-header" @click.stop="cardClicked">{{ header }}</div>
        <img :src="image" :alt="header" class="card-image">
      </div>
      <div class="back">
        <div class="card-header">{{ header }}</div>
        <p>{{ cardText }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    header: {
      type: String,
      required: true
    },
    image: {
      type: String,
      required: true
    },
    textPath: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      isFlipped: false,
      cardText: ''
    };
  },
  created() {
    this.fetchText();
  },
  methods: {
    flipCard() {
      this.isFlipped = !this.isFlipped;
    },
    async fetchText() {
      this.cardText = this.textPath;
    },
    cardClicked() {
      this.$emit('card-clicked');
    }
  }
};
</script>

<style scoped>
.flip-card {
  perspective: 1000px;
  width: 300px;
  margin: 0 15px;
  cursor: pointer;
}

.flip-card>div {
  width: 100%;
  height: 400px;
  text-align: center;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  position: relative;
  border: 1px solid #ddd;
  border-radius: 10px;
}

.flip-card .flipped {
  transform: rotateY(180deg);
}

.flip-card .front,
.flip-card .back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.flip-card .back {
  transform: rotateY(180deg);
  background-color: #f4f4f4;
  padding: 20px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  text-align: left;
  line-height: 1.6;
}

.flip-card .card-header {
  font-size: 1.2em;
  font-weight: bold;
  margin-bottom: 10px;
  padding: 10px;
}

.flip-card .card-image {
  max-width: 100%;
  max-height: 250px;
  object-fit: cover;
  border-radius: 10px;
}

.flip-card .back p {
  max-width: 90%;
  word-wrap: break-word;
  hyphens: auto;
}
</style>