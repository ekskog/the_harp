<template>
  <div v-if="selectedAccordeonId">
    <h2>Album Songs</h2>

    <!-- Dynamically display songs for the selected album -->
    <div v-for="song in getSelectedAlbumSongs" :key="song.title" class="song">
      <h3>{{ song.title }}</h3>
      <img :src="song.image" alt="song image" />
      <audio :src="song.audio" controls></audio>
      <p>{{ song.lyrics }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    selectedAccordeonId: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      albumData: [], // We'll populate this after fetching
    };
  },
  computed: {
    // Dynamically return songs for the selected album
    getSelectedAlbumSongs() {
      const selectedAlbum = this.albumData.find(
        (album) => album.id === this.selectedAccordeonId
      );
      return selectedAlbum ? selectedAlbum.songs : [];
    },
  },
  mounted() {
    // Fetch album data from external JSON file or API
    this.loadAlbumData();
  },
  methods: {
    async loadAlbumData() {
      try {
        const response = await fetch("/albums/albums.json");
        if (!response.ok) {
          throw new Error("Failed to load albums data");
        }
        this.albumData = await response.json();
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped>
.song {
  margin-bottom: 20px;
}

.song img {
  width: 100px;
  height: 100px;
}

.song audio {
  margin-top: 10px;
}

.song p {
  margin-top: 5px;
}
</style>
