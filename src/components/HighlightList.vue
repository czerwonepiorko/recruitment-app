<template>
  <div>
    <h2>Highlights</h2>
    <ul>
      <li v-for="highlight in highlights" :key="highlight.id">
        {{ highlight.text }}
        <button @click="removeHighlight(highlight.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HighlightList',
  props: ['highlights'],
  methods: {
    removeHighlight(id) {
      this.$emit('removeHighlight', id);
    }
  }
}
</script>
