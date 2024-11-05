<template>
  <div class="social-share wrapper">
    <a :href="telegramLink" target="_blank">Share to Telegram</a>
    <a :href="twitterLink" target="_blank">Share to Twitter</a>
  </div>
</template>

<script>
export default {
  props: {
    quote: Object,
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
};
</script>