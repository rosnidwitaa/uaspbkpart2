<template>
  <q-page class="container">
    <h1>ALBUM PHOTO</h1>
    <div class="photo-list">
      <q-card v-for="photo in photos" :key="photo.id" class="photo-card">
        <q-card-section class="q-pa-md">
          <q-img :src="photo.thumbnailUrl" class="thumbnail" />
        </q-card-section>
        <q-card-section class="q-pt-none q-pb-md">
          <q-item-label class="photo-title">{{ photo.title }}</q-item-label>
          <q-btn label="Lihat Foto" @click="viewPhoto(photo.url)" class="view-button" />
        </q-card-section>
      </q-card>
    </div>
    <q-dialog v-model="isPhotoDialogOpen" class="photo-dialog" persistent>
      <q-btn @click="closePhotoDialog" icon="close" class="close-button" />
      <img :src="currentPhotoUrl" class="full-photo" @click.stop />
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

    const closePhotoDialog = () => {
      isPhotoDialogOpen.value = false;
      currentPhotoUrl.value = ''; // Reset current photo URL
    };

    return {
      photos,
      isPhotoDialogOpen,
      currentPhotoUrl,
      viewPhoto,
      closePhotoDialog
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
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.photo-card {
  width: 300px; /* Lebar masing-masing card */
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
}

.photo-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.thumbnail {
  width: 100%;
  height: auto;
  border-radius: 8px 8px 0 0;
}

.photo-title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.view-button {
  background-color: #3498db;
  color: #ffffff;
  border-radius: 0;
}

.view-button:hover {
  background-color: #2980b9;
}

.photo-dialog {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  padding: 20px;
}

.full-photo {
  max-width: 100%;
  max-height: 80vh;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  color: #777;
  z-index: 1; /* Membuat tombol silang tetap di atas gambar */
}
</style>
