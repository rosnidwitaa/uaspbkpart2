<template>
    <div class="container">
      <h2>Posts</h2>
      <div class="user-select">
        <label for="userSelect">Pilih User:</label>
        <select id="userSelect" v-model="selectedUserId" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <ul v-if="posts.length" class="post-list">
        <li v-for="post in posts" :key="post.id" class="post-item">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </li>
      </ul>
      <p v-else class="no-posts">Tidak ada post yang tersedia.</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUserId: null,
        posts: []
      };
    },
    methods: {
      fetchUsers() {
        fetch('https://jsonplaceholder.typicode.com/users')
          .then(response => response.json())
          .then(data => {
            this.users = data;
            if (this.users.length) {
              this.selectedUserId = this.users[0].id;
              this.fetchPosts();
            }
          });
      },
      fetchPosts() {
        if (this.selectedUserId) {
          fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
            .then(response => response.json())
            .then(data => {
              this.posts = data;
            });
        }
      }
    },
    mounted() {
      this.fetchUsers();
    }
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 700px;
    margin: 20px auto;
    padding: 30px;
    background: linear-gradient(145deg, #f9f9f9, #e9e9e9);
    box-shadow: 8px 8px 16px #d1d1d1, -8px -8px 16px #ffffff;
    border-radius: 12px;
  }
  
  h2 {
    font-size: 28px;
    margin-bottom: 20px;
    color: #444;
    text-align: center;
    text-transform: uppercase;
  }
  
  .user-select {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  label {
    font-size: 16px;
    font-weight: bold;
    color: #666;
    margin-bottom: 10px;
  }
  
  select {
    width: 80%;
    padding: 12px;
    font-size: 16px;
    border: none;
    border-radius: 8px;
    background-color: #f3f3f3;
    box-shadow: inset 2px 2px 5px #bababa, inset -2px -2px 5px #ffffff;
    outline: none;
  }
  
  .post-list {
    list-style: none;
    padding: 0;
  }
  
  .post-item {
    margin-bottom: 20px;
    padding: 20px;
    background: linear-gradient(145deg, #e6e6e6, #ffffff);
    box-shadow: 8px 8px 16px #d1d1d1, -8px -8px 16px #ffffff;
    border-radius: 8px;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .post-item:hover {
    transform: translateY(-5px);
    box-shadow: 12px 12px 24px #d1d1d1, -12px -12px 24px #ffffff;
  }
  
  h3 {
    font-size: 20px;
    margin-bottom: 10px;
    color: #333;
  }
  
  p {
    font-size: 16px;
    color: #777;
  }
  
  .no-posts {
    text-align: center;
    font-size: 18px;
    color: #999;
  }
  </style>
  