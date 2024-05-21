<template>
  <div id="app" class="app-container">
    <Device/>
  </div>
</template>

<script>
import Device from './components/Device.vue';

export default {
  components: {
    Device
  }
};
</script>

<style>
.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  margin: 0;
}
</style>
