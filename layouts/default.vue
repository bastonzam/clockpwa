<template>
  <v-app>
    <v-app-bar fixed app color="#4C4948">
      <v-toolbar-title :style="{ color: '#fff' }" v-text="title" />
    </v-app-bar>
    <Nuxt />
    <v-footer app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {
      title: "Clock",
    };
  },
};
</script>
