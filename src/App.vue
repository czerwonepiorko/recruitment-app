<template>
  <div id="app">
    <AddHighlight @highlightAdded="addHighlight"/>
    <HighlightList :highlights="highlights" @removeHighlight="removeHighlight"/>
    <DocumentViewer :document="document" :highlights="highlights"/>
  </div>
</template>

<script>
import DocumentViewer from './components/DocumentViewer.vue'
import AddHighlight from './components/AddHighlight.vue'
import HighlightList from './components/HighlightList.vue'

export default {
  name: 'App',
  components: {
    DocumentViewer,
    AddHighlight,
    HighlightList
  },
  data() {
    return {
      document: [],
      highlights: JSON.parse(localStorage.getItem('highlights')) || []
    }
  },
  created() {
    fetch('/plik.json')
      .then(response => response.json())
      .then(data => {
        this.document = data;
        console.log('Document loaded:', this.document);
      })
      .catch(error => {
        console.error('Error loading document:', error);
      });
  },
  methods: {
    addHighlight(highlight) {
      this.highlights.push(highlight);
      this.saveHighlights();
    },
    removeHighlight(highlightId) {
      this.highlights = this.highlights.filter(h => h.id !== highlightId);
      this.saveHighlights();
    },
    saveHighlights() {
      localStorage.setItem('highlights', JSON.stringify(this.highlights));
    }
  }
}
</script>
