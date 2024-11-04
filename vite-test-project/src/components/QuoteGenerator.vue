<template>
  <div class="quote-generator">
    <p v-if="error" class="error-message">{{ error }}</p>
    <div v-else>
      <p class="quote">{{ quote?.content }}</p>
      <p v-if="quote?.author" class="author">— {{ quote.author }}</p>

      <div class="wrapper">
        <button @click="copyToClipboard" class="copy-btn">Скопировать цитату</button>
        <button @click="fetchQuote" class="new-quote-btn">Получить новую цитату</button>
      </div>

      <!-- buttons for socials -->
      <div class="social-share wrapper">
        <a :href="telegramLink" target="_blank">Поделиться в Telegram</a>
        <a :href="facebookLink" target="_blank">Поделиться в Facebook</a>
      </div>
    </div>

    <h3>История цитат</h3>
    <ul class="history">
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
    };
  },
  computed: {
    telegramLink() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      return `https://t.me/share/url?url=${encodeURIComponent(text)}`;
    },
    facebookLink() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      return `https://www.facebook.com/share/share.php?u=${encodeURIComponent(text)}`;
    },
  },
  methods: {
    async fetchQuote() {
      try {
        this.error = null;
        const response = await axios.get('https://api.quotable.io/random');
        this.quote = response.data;
        this.history.unshift(response.data); // добавляем цитату в начало истории
      } catch (err) {
        this.error = 'Произошла ошибка при загрузке цитаты. Попробуйте позже.';
      }
    },
    copyToClipboard() {
      const text = `"${this.quote?.content}" — ${this.quote?.author}`;
      navigator.clipboard.writeText(text).then(() => {
        alert("Цитата скопирована в буфер обмена!");
      });
    },
  },
  mounted() {
    this.fetchQuote();
  },
};
</script>

<style>
.wrapper {
  display: flex;
  align-content: center;
  justify-content: center;
  gap: 10px;
}

.social-share {
  margin: 15px 0;
}

.history-item {
  width: max-content;
}
</style>
