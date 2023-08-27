<script lang="ts">
import { defineComponent } from 'vue';
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default defineComponent({
  components: {
    Header,
    Main,
  },
});
</script>

<template>
  <div>
    <Header />
    <Main />
  </div>
</template>

<style scoped></style>
