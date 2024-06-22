<template>
    <q-page class="container">
      <h1>ALBUMS</h1>
      <q-list bordered class="album-list">
        <q-item v-for="album in albums" :key="album.id" clickable @click="goToAlbum(album.id)" class="album-item">
          <q-item-section>
            <q-item-label class="album-title">{{ album.title }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-page>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'Albums',
    setup() {
      const albums = ref([]);
      const router = useRouter();
  
      const fetchAlbums = async () => {
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/albums');
          albums.value = response.data;
        } catch (error) {
          console.error('Error fetching albums:', error);
        }
      };
  
      const goToAlbum = (id) => {
        router.push({ name: 'AlbumPhotos', params: { id } });
      };
  
      onMounted(fetchAlbums);
  
      return {
        albums,
        goToAlbum,
      };
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 900px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    text-align: center;
    font-size: 28px;
    color: #333;
    margin-bottom: 20px;
  }
  
  .album-list {
    background-color: #ffffff;
    border-radius: 8px;
  }
  
  .album-item {
    display: flex;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #e0e0e0;
    transition: background-color 0.3s ease, transform 0.3s ease;
    cursor: pointer;
  }
  
  .album-item:hover {
    background-color: #f0f0f0;
    transform: translateY(-2px);
  }
  
  .album-title {
    font-size: 18px;
    font-weight: bold;
    color: #333;
  }
  </style>
  