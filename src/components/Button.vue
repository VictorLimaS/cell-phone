<template>
  <div class="button" @click="handleClick">
    <span>{{ number }}</span>
    <small>{{ letters }}</small>
  </div>
</template>

<script>
export default {
  props: {
    number: {
      type: String,
      required: true
    },
    letters: {
      type: String,
      required: true
    }
  },
  methods: {
    handleClick() {
      this.$emit('click', this.number);
    }
  }
};
</script>

<style scoped>

.button span {
  font-size: 30px;
}
  
.button small {
  font-size: 17px;
}
</style>
