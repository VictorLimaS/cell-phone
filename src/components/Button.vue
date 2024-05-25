<template>
  <div class="button" @click="handleClick">
    <div class="number">{{ number }}</div>
    <div class="letters">{{ letters }}</div>
  </div>
</template>

<script>
export default {
  name: 'Button',
  props: {
    number: {
      type: [String, Number],
      required: true,
    },
    letters: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleClick() {
      this.$emit('click', this.number); 
    },
  },
};
</script>

<style scoped>
.button {
  width: 80px;
  height: 80px;
  background: #0e0e0e;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 24px;
  flex-direction: column;
  transition: background-color 0.1s ease;
}

.button .number {
  font-size: 32px;
}

.button .letters {
  font-size: 14px;
  margin-top: -4px;
}

.button:active {
  background: #565656;
}
</style>
