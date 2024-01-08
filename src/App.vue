<template>
  <div id="app" class="gradient-background">
    <NavBar />
    <MapWorld />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue';
import MapWorld from './components/MapWorld.vue';

export default {
  name: 'App',
  components: {
    NavBar,
    MapWorld,
},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  position: center;
  overflow: hidden;
}

#app input {
  color: #ffffff;
}

.gradient-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -2;
  background: linear-gradient(45deg, #b45d42, #c5336b, #eb7953, #1a94ba, #1cc7a2, #5ef675);
  background-size: 200% 200%;
  animation: moveGradient 50s linear infinite;
}

@keyframes moveGradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>
