<template>
    <div class="carousel-container">
        <div class="carousel-wrapper">
            <button class="carousel-control left" @click="scrollLeft">‹</button>
            <div class="carousel" ref="carousel">
                <!-- Add duplicate cards at the end and beginning for smooth transition -->
                <FlipCard v-for="card in displayCards" :key="card.uniqueId" :header="card.header" :image="card.image"
                    :text-path="card.textPath" :is-flipped="card.isFlipped" @flip="handleCardFlip(card.id)" />
            </div>
            <button class="carousel-control right" @click="scrollRight">›</button>
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
            currentIndex: 0,
            cards: [
                { id: 1, header: 'MaMoM', image: new URL('/images/card1.jpg', import.meta.url).href, textPath: card1Text, isFlipped: false },
                { id: 2, header: 'V2', image: new URL('/images/card2.jpg', import.meta.url).href, textPath: card2Text, isFlipped: false },
                { id: 3, header: 'DLU', image: new URL('/images/card3.jpg', import.meta.url).href, textPath: card3Text, isFlipped: false },
                { id: 4, header: 'SaNDGoDS', image: new URL('/images/card4.jpg', import.meta.url).href, textPath: card4Text, isFlipped: false },
                { id: 5, header: 'THE NeWSFeeD', image: new URL('/images/card5.jpg', import.meta.url).href, textPath: card5Text, isFlipped: false },
                { id: 6, header: 'DeaD/NoT DeaD', image: new URL('/images/card6.jpg', import.meta.url).href, textPath: card6Text, isFlipped: false },
            ],
        };
    },
    computed: {
        displayCards() {
            // Create a circular array of cards
            const totalCards = this.cards.length;
            return this.cards.map((card, index) => ({
                ...card,
                uniqueId: `${card.id}-${index}` // Ensure unique keys for Vue
            }));
        }
    },
    mounted() {
        // Check dimensions on load to ensure everything is sized correctly
        const carousel = this.$refs.carousel;
        console.log("Carousel width:", carousel.offsetWidth);
        const cards = carousel.children;
        Array.from(cards).forEach((card, index) => {
            console.log(`Card ${index + 1} width:`, card.offsetWidth);
        });
        // Add scroll event listener
        carousel.addEventListener('scroll', this.handleScroll);
    },
    beforeDestroy() {
        // Clean up scroll listener
        this.$refs.carousel.removeEventListener('scroll', this.handleScroll);
    },
    methods: {
        scrollRight() {
        const carousel = this.$refs.carousel;
        const cardWidth = carousel.offsetWidth / 3;
        const maxScroll = carousel.scrollWidth - carousel.offsetWidth;
        const currentScroll = carousel.scrollLeft;

        // If we're at or near the end
        if (currentScroll >= maxScroll - 10) {  // Adding small buffer for rounding
            // Jump to the beginning
            carousel.scrollTo({ left: 0, behavior: 'smooth' });
        } else {
            carousel.scrollBy({ left: cardWidth, behavior: 'smooth' });
        }
    },

    scrollLeft() {
        const carousel = this.$refs.carousel;
        const cardWidth = carousel.offsetWidth / 3;
        const currentScroll = carousel.scrollLeft;

        // If we're at or near the beginning
        if (currentScroll <= 10) {  // Adding small buffer for rounding
            // Jump to the end
            carousel.scrollTo({ 
                left: carousel.scrollWidth - carousel.offsetWidth, 
                behavior: 'smooth' 
            });
        } else {
            carousel.scrollBy({ left: -cardWidth, behavior: 'smooth' });
        }
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
    max-width: 1200px;
    /* Increased to accommodate 3 cards comfortably */
    margin: 0 auto;
    padding: 0 20px;
    /* Added padding for better spacing */
    border: black 1px solid;

}

.carousel-wrapper {
    overflow: hidden;
    width: 100%;
    padding: 20px 0;
    position: relative;
    /* Added for absolute positioning of buttons */
    display: flex;
    align-items: center;
    gap: 10px;
}


.carousel {
    display: flex;
    gap: 20px;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    scrollbar-width: none;
    -ms-overflow-style: none;
    scroll-behavior: smooth;
}

.carousel::-webkit-scrollbar {
    display: none;
    /* Hides scrollbar in Chrome/Safari */
}

.carousel-control {
    padding: 20px 15px;
    background-color: #f4f4f4;
    border: 1px solid #ddd;
    cursor: pointer;
    font-size: 24px;
    z-index: 1;
    transition: background-color 0.3s;
}

.carousel-control:hover {
    background-color: #e0e0e0;
}

.carousel-control:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.carousel-control button {
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
</style>