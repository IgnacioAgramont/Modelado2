<template>
  <NavHeader />
  <router-view />
</template>

<script>
import NavHeader from './components/NavHeader.vue'

export default {
  name: 'App',
  components: {
    NavHeader,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Poppins');

body {
  font-family: 'Poppins', sans-serif;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  background-attachment: fixed;
  z-index: -1;
  position: relative;
}

* {
  margin: 0;
  padding: 0;
}

i {
  margin-right: 10px;
}

NavHeader {
  z-index: 1;
  position: relative;
}
</style>