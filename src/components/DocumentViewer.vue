<template>
  <div>
    <div v-for="item in document" :key="item.id" class="document-section">
      <p v-html="applyHighlights(item.text, item.id)"></p>
    </div>
  </div>
</template>

<script>
import Mark from 'mark.js';

export default {
  name: 'DocumentViewer',
  props: ['document', 'highlights'],
  watch: {
    highlights: {
      handler(newHighlights) {
        this.applyHighlightsToDOM(newHighlights);
      },
      deep: true,
      immediate: true
    }
  },
  mounted() {
    this.applyHighlightsToDOM(this.highlights);
  },
  methods: {
    applyHighlightsToDOM(highlights) {
      this.$nextTick(() => {
        const context = this.$el;
        const instance = new Mark(context);
        instance.unmark({
          done: () => {
            highlights.forEach(h => {
              instance.mark(h.text, {
                className: 'highlight',
                separateWordSearch: false,
                acrossElements: true,
                diacritics: true,
                caseSensitive: false
              });
            });
          }
        });
      });
    },
    applyHighlights(text, documentId) {
      const highlights = this.highlights.filter(h => h.documentId === documentId);
      
      highlights
        .sort((a, b) => b.text.length - a.text.length)
        .forEach(h => {
          const escapedText = this.escapeRegExp(h.text);
          const regex = new RegExp(`(${escapedText})`, 'gi');
          text = text.replace(regex, '<span style="background-color: red;">$1</span>');
        });

      return text;
    },
    escapeRegExp(string) {
      return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    }
  }
}
</script>

<style scoped>
.highlight {
  background-color: red;
  color: white;
}
</style>
