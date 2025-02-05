<template>
    <div class="lyrics-container">
      <div v-if="selectedLyrics" class="lyrics-panel">
        <div class="lyrics-header">
          <h2>{{ selectedLyrics.title }}</h2>
          <button @click="closeLyrics" class="close-button">×</button>
        </div>
        <div class="lyrics-content">
          <pre>{{ selectedLyrics.lyrics }}</pre>
        </div>
      </div>
      <div v-else class="no-lyrics-selected">
        <p>Click on a card to view lyrics</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      lyrics: {
        type: Array,
        default: () => []
      }
    },
    data() {
      return {
        selectedLyrics: null
      };
    },
    methods: {
      displayLyrics(lyricsId) {
        this.selectedLyrics = this.lyrics.find(l => l.id === lyricsId);
      },
      closeLyrics() {
        this.selectedLyrics = null;
      }
    }
  }
  </script>
  
  <style scoped>
  .lyrics-container {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px 15px;
  }
  
  .lyrics-panel {
    background-color: #f4f4f4;
    border-radius: 10px;
    border: 1px solid #ddd;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    animation: slideIn 0.3s ease-out;
    border: black 1px solid;
  }
  
  .lyrics-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border-bottom: 1px solid #ddd;
    background-color: #e9e9e9;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
  
  .lyrics-header h2 {
    margin: 0;
    font-size: 1.2rem;
  }
  
  .close-button {
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    color: #666;
    transition: color 0.2s;
  }
  
  .close-button:hover {
    color: #000;
  }
  
  .lyrics-content {
    padding: 20px;
    max-height: 400px;
    overflow-y: auto;
  }
  
  .lyrics-content pre {
    white-space: pre-wrap;
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
    margin: 0;
  }
  
  .no-lyrics-selected {
    text-align: center;
    color: #888;
    padding: 20px;
  }
  
  @keyframes slideIn {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  @media screen and (max-width: 768px) {
    .lyrics-container {
      padding: 10px;
    }
  
    .lyrics-header {
      padding: 10px;
    }
  
    .lyrics-content {
      padding: 15px;
      max-height: 300px;
    }
  }
  </style>