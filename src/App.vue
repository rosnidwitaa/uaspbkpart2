<template>
  <q-layout view="hHh lpR fFf" class="navbar">
    <q-header>
      <q-toolbar>
        <div class="right-section">
          <q-btn flat dense label="Todos" @click="navigateTo('/todos')" />
          <q-btn flat dense label="Posts" @click="navigateTo('/posts')" />
          <q-btn flat dense label="Albums" @click="navigateTo('/albums')" />
        </div>
        <div class="spacer"></div>
        <div class="left-section">
          <span class="name">ROSNIDWITA</span>
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { useRouter } from 'vue-router';

export default {
  name: 'App',
  setup() {
    const router = useRouter();

    const navigateTo = (path) => {
      router.push(path);
    };

    return {
      navigateTo,
    };
  },
};
</script>

<style lang="sass">
@import 'quasar/src/css/index.sass'

.navbar
  background-color: #1a237e /* Dark blue background color */

.center-section
  display: flex
  align-items: center
  flex: 1

.right-section
  display: flex
  align-items: center
  justify-content: flex-end
  padding-right: 10px

.name
  color: #ffffff /* White text color */
  font-weight: bold
  font-size: 18px /* Adjust font size as needed */

.spacer
  flex: 1 /* Spacer to push "ROSNIDWITA" to the right */
</style>
