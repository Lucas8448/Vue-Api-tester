<script setup>
</script>

<template>
  <div class="container">
    <input v-model="apiEndpoint" type="text" placeholder="API Endpoint" />
    <div v-for="n in numParams" :key="n" class="param-container">
      <label>Parameter {{ n }}</label>
      <input v-model="params[n - 1].key" type="text" placeholder="Parameter name" />
      <input v-model="params[n - 1].value" type="text" placeholder="Parameter value" />
    </div>
    <button @click="addParam">Add Parameter</button>
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
      numParams: 0,
      params: [],
      response: null,
      requestHistory: []
    }
  },
  methods: {
    addParam() {
      this.numParams++;
      this.params.push({
        key: '',
        value: ''
      });
    },
    async makeRequest() {
      // build the params object from the user's input
      const requestParams = {};
      this.params.forEach(param => {
        requestParams[param.key] = param.value;
      });

      try {
        const response = await axios.get('https://cors-anywhere.herokuapp.com/' + this.apiEndpoint, {
          params: requestParams
        });
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


