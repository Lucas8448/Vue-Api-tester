<script setup>
</script>

<template>
  <div class="container">
    <input v-model="apiEndpoint" type="text" placeholder="API Endpoint" />
    <button @click="makeRequest">Make Request</button>
    <p>{{ response }}</p>
    <h2>Request History</h2>
    <ul>
      <li v-for="request in requestHistory" :key="request.endpoint">
        <strong>Endpoint:</strong> {{ request.endpoint }}<br />
        <strong>Response:</strong> {{ request.response }}
      </li>
    </ul>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiEndpoint: '',
      response: null,
      requestHistory: []
    }
  },
  methods: {
    async makeRequest() {
      try {
        const response = await axios.get('https://cors-anywhere.herokuapp.com/' + this.apiEndpoint);
        this.response = response.data;
        this.requestHistory.push({
          endpoint: this.apiEndpoint,
          response: this.response
        });
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>

<style scoped>
</style>

