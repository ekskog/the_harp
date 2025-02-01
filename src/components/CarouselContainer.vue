<template>
    <div class="carousel-container">
        <div class="carousel-wrapper">
            <div class="carousel" ref="carousel">
                <FlipCard v-for="card in cards" :key="card.id" :header="card.header" :image="card.image"
                    :text-path="card.textPath" :is-flipped="card.isFlipped" @flip="handleCardFlip(card.id)" />
            </div>
        </div>
        <div class="carousel-controls">
            <button @click="scrollLeft">‹</button>
            <button @click="scrollRight">›</button>
        </div>
    </div>
</template>

<script>
import FlipCard from './FlipCard.vue';
import card1Text from '@/assets/texts/card1.txt?raw';
import card2Text from '@/assets/texts/card2.txt?raw';
import card3Text from '@/assets/texts/card3.txt?raw';
import card4Text from '@/assets/texts/card4.txt?raw';
import card5Text from '@/assets/texts/card5.txt?raw';
import card6Text from '@/assets/texts/card6.txt?raw';

export default {
    components: { FlipCard },
    data() {
        return {
            cards: [
                { id: 1, header: 'MaMoM', image: new URL('@/assets/images/card1.jpg', import.meta.url).href, textPath: card1Text, isFlipped: false },
                { id: 2, header: 'V2', image: new URL('@/assets/images/card2.jpg', import.meta.url).href, textPath: card2Text, isFlipped: false },
                { id: 3, header: 'DLU', image: new URL('@/assets/images/card3.jpg', import.meta.url).href, textPath: card3Text, isFlipped: false },
                { id: 4, header: 'SaNDGoDS', image: new URL('@/assets/images/card4.jpg', import.meta.url).href, textPath: card4Text, isFlipped: false },
                { id: 5, header: 'THE NeWSFeeD', image: new URL('@/assets/images/card5.jpg', import.meta.url).href, textPath: card5Text, isFlipped: false },
                { id: 6, header: 'DeaD/NoT DeaD', image: new URL('@/assets/images/card6.jpg', import.meta.url).href, textPath: card6Text, isFlipped: false },
            ],
        };
    },
    mounted() {
        // Check dimensions on load to ensure everything is sized correctly
        const carousel = this.$refs.carousel;
        console.log("Carousel width:", carousel.offsetWidth);
        const cards = carousel.children;
        Array.from(cards).forEach((card, index) => {
            console.log(`Card ${index + 1} width:`, card.offsetWidth);
        });
    },
    methods: {
        scrollLeft() {
            const carousel = this.$refs.carousel;
            console.log("Current scroll position (left):", carousel.scrollLeft);
            carousel.scrollBy({ left: -300, behavior: 'smooth' });
            console.log("New scroll position (left):", carousel.scrollLeft);
        },
        scrollRight() {
            const carousel = this.$refs.carousel;
            console.log("Current scroll position (right):", carousel.scrollLeft);
            carousel.scrollBy({ left: 300, behavior: 'smooth' });
            console.log("New scroll position (right):", carousel.scrollLeft);
        },
        handleCardFlip(cardId) {
            this.cards = this.cards.map(card => ({
                ...card,
                isFlipped: card.id === cardId ? !card.isFlipped : false
            }));
        },
    },
};
</script>

<style scoped>
.carousel-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    max-width: 900px;
    margin-top: 50px;
    margin: 0 auto;
    overflow: hidden;
    
}

.carousel-wrapper {
    overflow: hidden;
    width: 100%;
}

.carousel {
    display: flex;
    gap: 20px;
    overflow: hidden;
}

.carousel-controls {
    display: flex;
    justify-content: center;
    margin-top: 100px;
}

.carousel-controls button {
    margin: 0 10px;
    padding: 10px;
    background-color: #f4f4f4;
    border: 1px solid #ddd;
    cursor: pointer;
}

.carousel button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.flip-card {
    flex: 0 0 calc(33.33% - 20px);
    box-sizing: border-box;
}
</style>