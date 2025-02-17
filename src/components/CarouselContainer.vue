<template>
  <div class="carousel-container">
    <div class="carousel-wrapper">
      <button class="carousel-control left" @click="scrollLeft">‹</button>
      <div class="carousel" ref="carousel">
        <FlipCard
          v-for="(card, index) in displayCards"
          :key="card.uniqueId"
          :header="card.header"
          :image="card.image"
          :text-path="card.textPath"
          :is-flipped="card.isFlipped"
          :card-number="index + 1"
          :card-id="card.uniqueId"
          :accordeon-id="card.id"
          @flip="() => handleCardFlip(card.id)"
          @show-accordeon="displayAccordeon" 
        />
      </div>
      <button class="carousel-control right" @click="scrollRight">›</button>
    </div>
    <!-- Conditionally display the AlbumAccordeon BELOW the carousel -->
    <div v-if="selectedAccordeonId" class="Accordeon-wrapper">
      <AlbumAccordeon :selectedAccordeonId="selectedAccordeonId" />
    </div>
  </div>
</template>

<script>
import FlipCard from "./FlipCard.vue";
import AlbumAccordeon from "./AlbumAccordeon.vue";

import card1Text from "/texts/card1.txt?raw";
import card2Text from "/texts/card2.txt?raw";
import card3Text from "/texts/card3.txt?raw";
import card4Text from "/texts/card4.txt?raw";
import card5Text from "/texts/card5.txt?raw";
import card6Text from "/texts/card6.txt?raw";

export default {
  components: {
    FlipCard,
    AlbumAccordeon,
  },
  data() {
    return {
      selectedAccordeonId: null,
      currentIndex: 0,
      cards: [
        {
          id: 1,
          header: "MaMoM",
          image: new URL("/images/card1.jpg", import.meta.url).href,
          textPath: card1Text,
          isFlipped: false,
        },
        {
          id: 2,
          header: "V2",
          image: new URL("/images/card2.jpg", import.meta.url).href,
          textPath: card2Text,
          isFlipped: false,
        },
        {
          id: 3,
          header: "DLU",
          image: new URL("/images/card3.jpg", import.meta.url).href,
          textPath: card3Text,
          isFlipped: false,
        },
        {
          id: 4,
          header: "SaNDGoDS",
          image: new URL("/images/card4.jpg", import.meta.url).href,
          textPath: card4Text,
          isFlipped: false,
        },
        {
          id: 5,
          header: "The NeWSFeeD",
          image: new URL("/images/card5.jpg", import.meta.url).href,
          textPath: card5Text,
          isFlipped: false,
        },
        {
          id: 6,
          header: "DeaD/NoT DeaD",
          image: new URL("/images/card6.jpg", import.meta.url).href,
          textPath: card6Text,
          isFlipped: false,
          lyrisId: 6,
        },
      ],
    };
  },
  computed: {
    displayCards() {
      // Create a circular array of cards
      return this.cards.map((card, index) => ({
        ...card,
        uniqueId: `${card.id}-${index}`, // Ensure unique keys for Vue
      }));
    },
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
    carousel.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    // Clean up scroll listener
    this.$refs.carousel.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    // Scroll methods remain exactly the same as in your original implementation
    scrollRight() {
      const carousel = this.$refs.carousel;
      const cardWidth = carousel.offsetWidth / 3;
      const maxScroll = carousel.scrollWidth - carousel.offsetWidth;
      const currentScroll = carousel.scrollLeft;

      // If we're at or near the end
      if (currentScroll >= maxScroll - 10) {
        // Adding small buffer for rounding
        // Jump to the beginning
        carousel.scrollTo({ left: 0, behavior: "smooth" });
      } else {
        carousel.scrollBy({ left: cardWidth, behavior: "smooth" });
      }
    },

    scrollLeft() {
      const carousel = this.$refs.carousel;
      const cardWidth = carousel.offsetWidth / 3;
      const currentScroll = carousel.scrollLeft;

      // If we're at or near the beginning
      if (currentScroll <= 10) {
        // Adding small buffer for rounding
        // Jump to the end
        carousel.scrollTo({
          left: carousel.scrollWidth - carousel.offsetWidth,
          behavior: "smooth",
        });
      } else {
        carousel.scrollBy({ left: -cardWidth, behavior: "smooth" });
      }
    },
    handleCardFlip(cardId) {
      // I've simplified the flip logic slightly
      this.cards = this.cards.map((card) => ({
        ...card,
        isFlipped: card.id === cardId ? !card.isFlipped : false,
      }));
    },
    // Optional: Add a scroll event handler if needed
    handleScroll() {
      // You can add any scroll-related logic here if necessary
    },
    displayAccordeon(cardId) {
      // Update selectedAccordeonId to the cardId, triggering the accordion to be displayed
      console.log("CarouselContainer: Displaying Accordeon for card ID", cardId);
      this.selectedAccordeonId = cardId;
    },
  },
};
</script>

<style scoped>
.carousel-container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 15px;
  box-sizing: border-box;
}

.carousel-wrapper {
  display: flex;
  align-items: center;
  gap: 20px;
  /* Space for arrows */
  width: 100%;
}

.carousel-control {
  background-color: rgba(244, 244, 244, 0.8);
  border: 1px solid #ddd;
  cursor: pointer;
  font-size: 24px;
  padding: 10px 15px;
  border-radius: 50%;
  transition: background-color 0.3s, transform 0.2s;
  flex-shrink: 0;
  /* Prevent arrows from shrinking */
}

.carousel-control:hover {
  background-color: #f0f0f0;
  transform: scale(1.1);
}

.carousel-control.left {
  order: -1;
  /* Place left arrow before carousel */
}

.carousel-control.right {
  order: 1;
  /* Place right arrow after carousel */
}

.carousel {
  flex-grow: 1;
  /* Allow carousel to take remaining space */
  display: flex;
  gap: 15px;
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

.flip-card {
  flex: 0 0 calc(33.333% - 14px);
  max-width: calc(33.333% - 14px);
  min-width: 250px;
}

/* Mobile Specific Styles */
@media screen and (max-width: 768px) {
  .carousel-wrapper {
    gap: 15px;
  }

  .carousel-control {
    font-size: 20px;
    padding: 8px 12px;
  }

  .carousel {
    gap: 10px;
  }

  .flip-card {
    flex: 0 0 calc(50% - 10px);
    max-width: calc(50% - 10px);
    min-width: 150px;
  }
}

/* Very Small Screens */
@media screen and (max-width: 480px) {
  .carousel-wrapper {
    gap: 10px;
  }

  .carousel-control {
    font-size: 18px;
    padding: 6px 10px;
  }

  .carousel {
    gap: 15px;
  }

  .flip-card {
    flex: 0 0 calc(100% - 10px);
    max-width: calc(100% - 10px);
    min-width: 100px;
  }
}

/* Ensure arrows are visible and centered */
@media screen and (max-width: 768px) {
  .carousel-control {
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
