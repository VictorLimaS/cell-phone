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
*{
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

html,
body {
  overflow: hidden;
}

#app {
  padding: 0;
}

</style>
