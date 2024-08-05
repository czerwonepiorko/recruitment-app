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
          documentId: 1, // Możesz dynamicznie ustawić ID dokumentu
          text: this.highlightText.trim()
        };

        // Emituj zdarzenie z nowym zaznaczeniem
        this.$emit('addHighlight', highlight);

        // Zresetuj pole tekstowe
        this.highlightText = '';
      }
    }
  }
}
</script>
