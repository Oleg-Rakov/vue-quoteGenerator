<template>
  <div>
    <div class="quote-wrapper">
      <p class="quote">{{ quote?.content }}</p>
      <p v-if="quote?.author" class="author">— {{ quote.author }}</p>
    </div>
    <div class="wrapper">
      <button @click="copyToClipboard" class="copy-btn">Copy quote</button>
      <button @click="fetchQuote" :disabled="isLoading" class="new-quote-btn">
        <span v-if="isLoading">Loading...</span>
        <span v-else>Get new quote</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    quote: Object,
    isLoading: Boolean,
  },
  emits: ['fetch-quote'],
  methods: {
    copyToClipboard() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      navigator.clipboard.writeText(text).then(() => {
        alert("The quote is copied to the clipboard!");
      });
    },
    fetchQuote() {
      this.$emit('fetch-quote');
    },
  },
};
</script>