<template>
  <div>
    <button
      v-for="index in 10"
      :key="index"
      @click="setRating(index)"
      :class="{ 'selected': index === currentRating }"
    >{{ index }}</button>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      required: true
    },
    releaseDate: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      currentRating: this.value
    };
  },
  mounted() {
    const storedRating = localStorage.getItem(this.releaseDate);
    if (storedRating) {
      this.currentRating = parseInt(storedRating);
    }
  },
  methods: {
    setRating(rating) {
      this.currentRating = rating;
      localStorage.setItem(this.releaseDate, rating);
      this.$emit('input', rating);
    }
  }
};
</script>

<style scoped>
button {
  cursor: pointer;
  border: none;
  background-color: transparent;
  font-size: 16px;
  margin: 0 5px;
  padding: 5px 10px;
}

button.selected {
  background-color: #007bff;
  color: #fff;
  border-radius: 5px;
}
</style>
