<template>
  <div id="app">
    <Header />
    <Earthquake/>
  </div>
</template>

<script>
import Earthquake from './components/Earthquake.vue'
import Header from './components/Header';

export default {
  name: 'App',
  components: {
    Earthquake,
    Header
  }
}
</script>

