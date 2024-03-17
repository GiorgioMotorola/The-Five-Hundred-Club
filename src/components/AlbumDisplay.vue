<template>
  <div>
    <h2>{{ currentAlbum.title }}</h2>
    <img :src="currentAlbum.image" alt="Album Cover">
    <AlbumRating :value="currentAlbum.rating" :releaseDate="currentAlbum.releaseDate" @input="updateRating" />
    <div v-if="nextAlbum" class="countdown">
      Next album available in: {{ countdown }}
    </div>
  </div>
</template>

<script>
import AlbumRating from './AlbumRating.vue';

export default {
  components: {
    AlbumRating
  },
  props: {
    albums: {
      type: Array,
      required: true
    },
    currentDate: {
      type: Date,
      required: true
    }
  },
  data() {
    return {
      countdown: '',
      nextAlbumIndex: 0,
      timer: null
    };
  },
  computed: {
    currentAlbum() {
      const today = this.currentDate.toISOString().split('T')[0];
      return this.albums.find(album => album.releaseDate === today);
    },
    nextAlbum() {
      return this.albums[this.nextAlbumIndex];
    }
  },
  methods: {
    updateRating(rating) {
      this.currentAlbum.rating = rating;
      localStorage.setItem(this.currentAlbum.releaseDate, rating);
    },
    startCountdown() {
  this.timer = setInterval(() => {
    const now = new Date();
    const nextReleaseDate = new Date(this.nextAlbum.releaseDate);

    nextReleaseDate.setTime(nextReleaseDate.getTime() - (nextReleaseDate.getTimezoneOffset() * 60000));

    const diff = nextReleaseDate.getTime() - now.getTime();

    if (diff <= 0) {
      clearInterval(this.timer);
      this.countdown = '';
    } else {
      const hours = Math.floor(diff / (1000 * 60 * 60));
      const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((diff % (1000 * 60)) / 1000);
      this.countdown = `${hours}h ${minutes}m ${seconds}s`;
    }
  }, 1000);
}
  },
  mounted() {
    const now = this.currentDate.getTime();
    for (let i = 0; i < this.albums.length; i++) {
      const releaseDate = new Date(this.albums[i].releaseDate).getTime();
      if (releaseDate > now) {
        this.nextAlbumIndex = i;
        break;
      }
    }

    if (this.nextAlbum) {
      this.startCountdown();
    }
  },
  beforeDestroy() {
    clearInterval(this.timer);
  }
};
</script>

<style scoped>
.countdown {
  margin-top: 10px;
  font-size: 18px;
}
</style>
