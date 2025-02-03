<template>
    <div class="carousel-container">
        <div class="carousel-wrapper">
            <button class="carousel-control left" @click="scrollLeft">‹</button>
            <div class="carousel" ref="carousel">
                <FlipCard 
                    v-for="card in displayCards" 
                    :key="card.uniqueId" 
                    :header="card.header" 
                    :image="card.image"
                    :text-path="card.textPath" 
                    :is-flipped="card.isFlipped" 
                    @flip="() => handleCardFlip(card.id)" 
                />
            </div>
            <button class="carousel-control right" @click="scrollRight">›</button>
        </div>
    </div>
</template>

<script>
import FlipCard from './FlipCard.vue';
import card1Text from '/texts/card1.txt?raw';
import card2Text from '/texts/card2.txt?raw';
import card3Text from '/texts/card3.txt?raw';
import card4Text from '/texts/card4.txt?raw';
import card5Text from '/texts/card5.txt?raw';
import card6Text from '/texts/card6.txt?raw';

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
        // Scroll methods remain exactly the same as in your original implementation
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
            // I've simplified the flip logic slightly
            this.cards = this.cards.map(card => ({
                ...card,
                isFlipped: card.id === cardId ? !card.isFlipped : false
            }));
        },
        // Optional: Add a scroll event handler if needed
        handleScroll() {
            // You can add any scroll-related logic here if necessary
        }
    },
};
</script>

<style scoped>
/* Your existing CSS remains the same */
</style>

<style scoped>
.carousel-container {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 15px; /* Reduced padding for mobile */
    box-sizing: border-box;
}

.carousel-wrapper {
    position: relative;
    width: 100%;
    display: flex;
    align-items: center;
    gap: 10px;
    overflow: hidden;
}

.carousel {
    display: flex;
    gap: 15px; /* Reduced gap for smaller screens */
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    scrollbar-width: none;
    -ms-overflow-style: none;
    scroll-behavior: smooth;
    width: 100%;
    padding: 10px 0;
}

.carousel::-webkit-scrollbar {
    display: none;
}

.carousel-control {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    z-index: 10;
    background-color: rgba(244, 244, 244, 0.8);
    border: 1px solid #ddd;
    cursor: pointer;
    font-size: 24px;
    padding: 10px 15px;
    border-radius: 50%;
    transition: background-color 0.3s;
}

.carousel-control.left {
    left: 0;
}

.carousel-control.right {
    right: 0;
}

/* Mobile Specific Styles */
@media screen and (max-width: 768px) {
    .carousel-container {
        padding: 0 10px;
    }

    .carousel {
        gap: 10px;
    }

    .flip-card {
        flex: 0 0 calc(50% - 10px); /* 2 cards per row on smaller screens */
        max-width: calc(50% - 10px);
        min-width: 150px; /* Smaller minimum width */
    }

    .carousel-control {
        font-size: 20px;
        padding: 8px 12px;
    }
}

/* Very Small Screens */
@media screen and (max-width: 480px) {
    .flip-card {
        flex: 0 0 calc(100% - 10px); /* 1 card per row on very small screens */
        max-width: calc(100% - 10px);
        min-width: 100px;
    }

    .carousel {
        gap: 15px;
    }

    .carousel-control {
        font-size: 18px;
        padding: 6px 10px;
    }
}
</style>