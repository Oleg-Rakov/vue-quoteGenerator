<template>
  <div v-if="history.length > 0">
    <h3>History quotes</h3>
    <ul class="history">
      <li class="history-item" v-for="(item, index) in history" :key="index">
        "{{ item.content }}" — {{ item.author }}
        <span class="delete-item" @click="removeQuote(index)">×</span>
      </li>
    </ul>
    <button @click="clearHistoryQuotes">Clear history</button>
  </div>
</template>

<script>
export default {
  props: {
    history: Array,
  },
  emits: ['remove-quote', 'clear-history'],
  methods: {
    removeQuote(index) {
      this.$emit('remove-quote', index);
    },
    clearHistoryQuotes() {
      this.$emit('clear-history');
    },
  },
};
</script>