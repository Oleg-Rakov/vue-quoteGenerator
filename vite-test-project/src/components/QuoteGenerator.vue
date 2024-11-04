<template>
  <div class="quote-generator">
    <div>
      <!--  If error, show error message  -->
      <div v-if="error" class="error-message">
        <span>{{ error }}</span>
        <button @click="fetchQuote" class="new-quote-btn">Get new quote</button>
      </div>

      <div v-else>
        <div v-if="quote">
          <div class="quote-wrapper">
            <p class="quote">{{ quote?.content }}</p>
            <p v-if="quote?.author" class="author">— {{ quote.author }}</p>
          </div>

          <div class="wrapper">
            <button @click="copyToClipboard" class="copy-btn">Copy quote</button>
            <button @click="fetchQuote" :disabled="isLoading" class="new-quote-btn">
              <!-- Showing text loading, if request sending -->
              <span v-if="isLoading">Loading...</span>
              <span v-else>Get new quote</span>
            </button>
          </div>

          <!-- buttons for socials -->
          <div class="social-share wrapper">
            <a :href="telegramLink" target="_blank">Share to Telegram</a>
            <a :href="twitterLink" target="_blank">Share to Twitter</a>
          </div>
        </div>
      </div>
    </div>

    <!--  History block  -->
    <h3 v-if="history.length > 0">History quotes</h3>
    <ul v-if="history.length > 0" class="history">
      <li class="history-item" v-for="(item, index) in history" :key="index">
        "{{ item.content }}" — {{ item.author }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      quote: null,
      error: null,
      history: [],
      isLoading: false,
    };
  },
  computed: {
    telegramLink() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      return `https://t.me/share/url?url=${encodeURIComponent(text)}`;
    },
    twitterLink() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      return `https://twitter.com/intent/tweet?text=${encodeURIComponent(text)}`;
    },
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
    copyToClipboard() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      navigator.clipboard.writeText(text).then(() => {
        alert("The quote is copied to the clipboard!");
      });
    },
  },
  mounted() {
    this.fetchQuote();
  },
};
</script>
