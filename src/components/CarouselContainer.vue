<template>
    <div class="carousel-container">
        <div class="carousel" ref="carousel">
            <FlipCard v-for="card in cards" :key="card.id" :header="card.header" :image="card.image"
                :text-path="card.textPath" />
        </div>
        <div class="carousel-controls">
            <button @click="scrollLeft">&lt;</button>
            <button @click="scrollRight">&gt;</button>
        </div>
    </div>
</template>

<script>
import card1Text from '@/assets/texts/card1.txt?raw'
import card2Text from '@/assets/texts/card2.txt?raw'
import card3Text from '@/assets/texts/card3.txt?raw'
import card4Text from '@/assets/texts/card4.txt?raw'
import card5Text from '@/assets/texts/card5.txt?raw'
import card6Text from '@/assets/texts/card6.txt?raw'

import FlipCard from './FlipCard.vue';

export default {
    components: {
        FlipCard
    },
    data() {
        return {
            cards: [
        {
          id: 1,
          header: 'MaMoM',
          image: new URL('@/assets/images/card1.jpg', import.meta.url).href,
          textPath: card1Text
        },
        {
          id: 2,
          header: 'V2',
          image: new URL('@/assets/images/card2.jpg', import.meta.url).href,
          textPath: card2Text
        },
        {
          id: 3,
          header: 'DLU',
          image: new URL('@/assets/images/card3.jpg', import.meta.url).href,
          textPath: card3Text
        },
        {
          id: 4,
          header: 'SaNDGoDS',
          image: new URL('@/assets/images/card4.jpg', import.meta.url).href,
          textPath: card4Text
        },
        {
          id: 5,
          header: 'THE NeWSFeeD',
          image: new URL('@/assets/images/card5.jpg', import.meta.url).href,
          textPath: card5Text
        },
        {
          id: 6,
          header: 'DeaD/NoT DeaD',
          image: new URL('@/assets/images/card6.jpg', import.meta.url).href,
          textPath: card6Text
        }
      ]
        };
    },
    methods: {
        scrollLeft() {
            this.$refs.carousel.scrollBy({
                left: -330,
                behavior: 'smooth'
            });
        },
        scrollRight() {
            this.$refs.carousel.scrollBy({
                left: 330,
                behavior: 'smooth'
            });
        }
    }
};
</script>

<style scoped>
.carousel-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
}

.carousel {
    display: flex;
    overflow-x: hidden;
    scroll-behavior: smooth;
    padding: 20px 0;
}

.carousel-controls {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

.carousel-controls button {
    margin: 0 10px;
    padding: 10px 20px;
    background-color: #f4f4f4;
    border: 1px solid #ddd;
    cursor: pointer;
}
</style>