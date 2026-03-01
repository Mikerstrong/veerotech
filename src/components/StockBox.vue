<script setup>
import { ref } from 'vue'

const symbol = 'ASTS'
const price = ref(null)
const loading = ref(false)
const error = ref('')

async function fetchStock() {
  loading.value = true;
  error.value = '';
  price.value = null;
  try {
    // Yahoo Finance unofficial public endpoint
    const url = `https://api.allorigins.win/get?url=${encodeURIComponent('https://query1.finance.yahoo.com/v7/finance/quote?symbols=' + symbol)}`;
    const res = await fetch(url);
    if (!res.ok) throw new Error('API error or symbol not found');
    const proxyData = await res.json();
    const data = JSON.parse(proxyData.contents);
    const quote = data.quoteResponse.result[0];
    if (!quote || !quote.regularMarketPrice) throw new Error('Symbol not found or no price available');
    price.value = quote.regularMarketPrice;
  } catch (e) {
    error.value = e.message;
  } finally {
    loading.value = false;
  }
}

fetchStock()
</script>

<template>
  <div class="stock-box">
    <h2>Latest Stock Price: {{ symbol }}</h2>
    <div v-if="loading">Loading...</div>
    <div v-if="error" class="error">{{ error }}</div>
    <div v-if="price !== null">
      <p>Current Price: ${{ price }}</p>
    </div>
    <button @click="fetchStock">Refresh</button>
  </div>
</template>

<style scoped>
.stock-box {
  max-width: 400px;
  margin: 2em auto;
  padding: 2em;
  border-radius: 8px;
  background: #f4f4f4;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
button {
  padding: 0.5em 1em;
  margin-top: 1em;
}
.error {
  color: #c00;
  margin-top: 1em;
}
</style>
