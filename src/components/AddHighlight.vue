<template>
  <div>
    <input type="text" v-model="highlightText" placeholder="Enter text to highlight"/>
    <button @click="addHighlight">Add Highlight</button>
  </div>
</template>

<script>
export default {
  name: 'AddHighlight',
  data() {
    return {
      highlightText: ''
    }
  },
  methods: {
    addHighlight() {
      if (this.highlightText.trim()) {
        const highlight = {
          id: Date.now(),
          documentId: 1,
          text: this.highlightText.trim()
        };

        let highlights = JSON.parse(localStorage.getItem('highlights')) || [];
        highlights.push(highlight);
        localStorage.setItem('highlights', JSON.stringify(highlights));

        this.highlightText = '';
        this.$emit('highlightAdded', highlight);
      }
    }
  }
}
</script>
