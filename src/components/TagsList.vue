<template>
  <div :class="['tags-list', alignmentClass]" ref="tagsList">
    <div v-for="(tag, index) in visibleTags" :key="index" class="tags-list__item">
      <v-icon>{{ tag.icon }}</v-icon>
      <v-icon v-if="index < visibleTags.length - 1">mdi-circle-small</v-icon>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TagsList',
  props: {
    tags: {
      type: Array,
      required: true
    },
    alignment: {
      type: String,
      default: 'left',
      validator: value => ['left', 'justify'].includes(value)
    }
  },

  data() {
    return {
      visibleTags: [],
    };
  },

  computed: {
    alignmentClass() {
      return this.alignment === 'justify' ? 'tags-list--justify' : 'tags-list--left';
    }
  },

  methods: {
    updateVisibleTags() {
      this.visibleTags = this.tags;

      this.$nextTick(() => {

        const wrapTags = this.$refs.tagsList;
        const tagsListWidth = this.$refs.tagsList.clientWidth;
        const tagItems = wrapTags.children;

        let totalWidth = 0;
        let countTags = 0;

        for (let i = 0; i < this.tags.length; i++) {
          const srtInnerItem = `<span class="tags-list__text">${this.tags[i].text}</span>`;

          if (tagItems[i]) {
            const text = tagItems[i].children[0];
            if (text && text.classList.contains('tags-list__text')) {
              text.remove();
            }

            tagItems[i].insertAdjacentHTML('afterbegin', srtInnerItem);

            if (tagItems[i].offsetWidth + totalWidth <= tagsListWidth) {
              countTags += 1;
              totalWidth += tagItems[i].offsetWidth;
            } else {
              while (tagItems[i] && i <= this.tags.length) {
                tagItems[i].remove();
                i++;
              }
              break;
            }
          }
        }

        this.visibleTags = this.tags.slice(0, countTags);
      });
    },
  },

  mounted() {
    this.updateVisibleTags();
    window.addEventListener('resize', this.updateVisibleTags);
  },

  beforeDestroy() {
    window.removeEventListener('resize', this.updateVisibleTags);
  },

  watch: {
    tags() {
      this.visibleTags = this.tags;
      this.updateVisibleTags();
    }
  }
};
</script>

<style lang="scss">
.tags-list {
  display: flex;
  overflow: hidden;
  width: 100%;

  &__item {
    padding: 0 4px;
    display: flex;
    justify-content: center;
    align-items: center;
    min-width: fit-content;
  }

  &--left {
    justify-content: flex-start;
  }

  &--justify {
    justify-content: space-between;
  }
}
</style>
