<template>
  <div id="app">
    <AlbumDisplay :albums="albums" :currentDate="currentDate" />
  </div>
</template>

<script>
import AlbumDisplay from './components/AlbumDisplay.vue';
import albumsData from './albums.json';

export default {
  components: {
    AlbumDisplay
  },
  data() {
    return {
      albums: albumsData,
      currentDate: new Date()
    };
  }
};
</script>
