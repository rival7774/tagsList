<template>
  <v-container>
    <TagsList :tags="tags" alignment="left"/>
    <TagsList :tags="tags" alignment="left" class="half-width"/>
    <TagsList :tags="tags" alignment="justify" class="half-width"/>
  </v-container>
</template>

<script>
import TagsList from './components/TagsList.vue';

export default {
  name: 'App',
  components: {
    TagsList
  },
  data() {
    return {
      tags: []
    };
  },
  created() {
    this.loadTags();
  },
  methods: {
    async loadTags() {
      try {
        const response = await fetch('/tags.json');
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        this.tags = await response.json();
      } catch (error) {
        console.error('Error loading tags:', error);
      }
    }
  }
};
</script>

<style scoped>
.half-width {
  width: 50%;
}
</style>
