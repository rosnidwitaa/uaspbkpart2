<template>
    <div class="todo-container">
      <h2>List Kegiatan</h2>
      <ul>
        <li v-for="kegiatan in kegiatanList" :key="kegiatan.id" :class="{ done: kegiatan.isDone }">
          <div class="task">
            <div class="task-info">
              <input type="checkbox" v-model="kegiatan.isDone" class="checkbox" />
              <span>{{ kegiatan.nama }}</span>
            </div>
            <button @click="deleteKegiatan(kegiatan)" class="delete-button">
              <i class="fas fa-trash-alt"></i>
            </button>
          </div>
        </li>
      </ul>
      <form @submit.prevent="addKegiatan" class="add-form">
        <input type="text" v-model="newKegiatan.nama" placeholder="Tambah Kegiatan Baru" class="input-text" />
        <button type="submit" class="add-button">
          <i class="fas fa-plus"></i> Tambah
        </button>
      </form>
      <button @click="showCompletedTasks" class="show-completed-button">
        <i class="fas fa-check"></i> Tampilkan Kegiatan yang Sudah Selesai
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        kegiatanList: [
          { id: 1, nama: 'berangkat futsal', isDone: false },
          { id: 2, nama: 'pulang kampung', isDone: false }
        ],
        newKegiatan: { nama: '' }
      };
    },
    methods: {
      addKegiatan() {
        if (this.newKegiatan.nama.trim()) {
          this.kegiatanList.push({ id: this.kegiatanList.length + 1, nama: this.newKegiatan.nama, isDone: false });
          this.newKegiatan.nama = '';
        }
      },
      deleteKegiatan(kegiatan) {
        this.kegiatanList = this.kegiatanList.filter(k => k.id !== kegiatan.id);
      },
      showCompletedTasks() {
        this.kegiatanList = this.kegiatanList.filter(kegiatan => kegiatan.isDone);
      }
    }
  };
  </script>
  
  <style scoped>
  .todo-container {
    max-width: 600px;
    margin: 40px auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    font-family: 'Roboto', sans-serif;
  }
  
  h2 {
    text-align: center;
    color: #3498db;
    font-size: 24px;
    margin-bottom: 20px;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    margin-bottom: 15px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  li:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  }
  
  .task {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
  }
  
  .task-info {
    display: flex;
    align-items: center;
  }
  
  .checkbox {
    margin-right: 15px;
    width: 20px;
    height: 20px;
    border-radius: 4px;
    cursor: pointer;
    appearance: none;
    background-color: #3498db;
    border: none;
    transition: background-color 0.3s;
  }
  
  .checkbox:checked {
    background-color: #2ecc71;
  }
  
  span {
    font-size: 18px;
    color: #333;
  }
  
  .delete-button {
    background: none;
    border: none;
    cursor: pointer;
    font-size: 18px;
    color: #e74c3c;
    transition: color 0.3s;
  }
  
  .delete-button:hover {
    color: #c0392b;
  }
  
  .add-form {
    display: flex;
    align-items: center;
    margin-top: 20px;
  }
  
  .input-text {
    flex: 1;
    padding: 12px;
    border: 2px solid #bdc3c7;
    border-radius: 5px;
    margin-right: 10px;
    font-size: 16px;
  }
  
  .add-button {
    padding: 12px 20px;
    background-color: #ecfd00;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 16px;
  }
  
  .add-button:hover {
    background-color: #2980b9;
  }
  
  .show-completed-button {
    margin-top: 20px;
    padding: 12px 20px;
    background-color: #ff0800;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    display: block;
    width: 100%;
    text-align: center;
    transition: background-color 0.3s;
    font-size: 16px;
  }
  
  .show-completed-button:hover {
    background-color: #27ae60;
  }
  </style>
  