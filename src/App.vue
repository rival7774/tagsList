<template>
  <v-container>
    <TagsList alignment="left" />
    <TagsList alignment="left" class="half-width" />
    <TagsList alignment="justify" class="half-width" />
  </v-container>
</template>

<script>
import TagsList from './components/TagsList.vue';

export default {
  name: 'App',

  components: {
    TagsList,
  },
};
</script>
