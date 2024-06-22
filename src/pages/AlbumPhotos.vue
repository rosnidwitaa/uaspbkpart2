<template>
    <q-page class="container">
      <h1>Album foto</h1>
      <q-list bordered class="photo-list">
        <q-item v-for="photo in photos" :key="photo.id" clickable @click="viewPhoto(photo.url)" class="photo-item">
          <q-item-section>
            <img :src="photo.thumbnailUrl" class="thumbnail" />
          </q-item-section>
          <q-item-section>
            <q-item-label class="photo-title">{{ photo.title }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
      <q-dialog v-model="isPhotoDialogOpen" class="photo-dialog">
        <img :src="currentPhotoUrl" class="full-photo" />
      </q-dialog>
    </q-page>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  
  export default {
    name: 'AlbumPhotos',
    props: {
      id: {
        type: Number,
        required: true
      }
    },
    setup(props) {
      const photos = ref([]);
      const isPhotoDialogOpen = ref(false);
      const currentPhotoUrl = ref('');
  
      const fetchPhotos = async () => {
        try {
          const response = await axios.get(`https://jsonplaceholder.typicode.com/photos?albumId=${props.id}`);
          photos.value = response.data;
        } catch (error) {
          console.error('Error fetching photos:', error);
        }
      };
  
      onMounted(fetchPhotos);
  
      const viewPhoto = (url) => {
        currentPhotoUrl.value = url;
        isPhotoDialogOpen.value = true;
      };
  
      return {
        photos,
        isPhotoDialogOpen,
        currentPhotoUrl,
        viewPhoto
      };
    }
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
  
  .photo-list {
    background-color: #ffffff;
    border-radius: 8px;
  }
  
  .photo-item {
    display: flex;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #e0e0e0;
    transition: background-color 0.3s ease, transform 0.3s ease;
    cursor: pointer;
  }
  
  .photo-item:hover {
    background-color: #f0f0f0;
    transform: translateY(-2px);
  }
  
  .thumbnail {
    width: 100px;
    height: 100px;
    object-fit: cover;
    border-radius: 4px;
    margin-right: 20px;
  }
  
  .photo-title {
    font-size: 18px;
    font-weight: bold;
    color: #333;
  }
  
  .photo-dialog {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
  }
  
  .full-photo {
    max-width: 100%;
    max-height: 80vh;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  </style>
  