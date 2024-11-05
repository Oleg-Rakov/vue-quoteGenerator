<template>
  <div class="quote-generator">
    <!--  If error, show error message  -->
    <div v-if="error" class="error-message">
      <span>{{ error }}</span>
      <button @click="fetchQuote" class="new-quote-btn">Get new quote</button>
    </div>

    <div v-else>
      <QuoteDisplay
          v-if="quote"
          :quote="quote"
          :isLoading="isLoading"
          @fetch-quote="fetchQuote"
      />

      <SocialShare v-if="quote" :quote="quote"/>

      <QuoteHistory
          :history="history"
          @remove-quote="removeQuote"
          @clear-history="clearHistoryQuotes"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SocialShare from './SocialShare.vue';
import QuoteDisplay from './QuoteDisplay.vue';
import QuoteHistory from "./QuoteHistory.vue";

export default {
  components: {QuoteDisplay, QuoteHistory, SocialShare},
  data() {
    return {
      quote: null,
      error: null,
      history: [],
      isLoading: false,
    };
  },
  methods: {
    async fetchQuote() {
      try {
        this.error = null;
        this.isLoading = true;
        const response = await axios.get('https://api.quotable.io/random');
        this.quote = response.data;
        this.history.unshift(response.data); // Add quote to first index of array
      } catch (err) {
        this.error = 'There was an error loading the quote. Please try again later.';
      } finally {
        this.isLoading = false; // Finish loading
      }
    },
    removeQuote(index) {
      this.history.splice(index, 1);
    },
    clearHistoryQuotes() {
      this.history = [];
    }
  },
  mounted() {
    this.fetchQuote();
  },
};
</script>
