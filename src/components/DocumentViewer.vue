<template>
  <div>
    <div v-for="item in document" :key="item.id">
      <p v-html="highlightText(item.text, item.id)"></p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DocumentViewer',
  props: ['document', 'highlights'],
  methods: {
    highlightText(text, documentId) {
      // Pobierz aktualne highlights
      const highlights = this.highlights.filter(h => h.documentId === documentId);
      
      // Zastosuj zaznaczenia
      highlights
        .sort((a, b) => b.text.length - a.text.length) // Większe zaznaczenia jako pierwsze
        .forEach(h => {
          // Użyj bezpiecznego regex dla dużych fragmentów
          const escapedText = this.escapeRegExp(h.text);
          const regex = new RegExp(`(${escapedText})`, 'gi');
          text = text.replace(regex, '<mark style="background-color: red;">$1</mark>');
        });

      return text;
    },
    escapeRegExp(string) {
      // Funkcja do uciekania specjalnych znaków w regex
      return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    }
  }
}
</script>
