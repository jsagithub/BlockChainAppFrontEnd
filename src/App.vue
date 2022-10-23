<template>
  <div id="app">
    <button v-if="!connected" @click="connect">Connect wallet</button>
    <button v-if="connected">Call contract</button>
    {{ contractResult }}
  </div>
</template>

<script>
import Web3 from 'web3'
export default {
  name: 'App',
  data () {
    return {
      connected: false,
      contractResult: ''
    }
  },

  methods: {

    connect: function () {
      // this connects to the wallet
      if (window.ethereum) { // first we check if metamask is installed
        window.ethereum.request({method: 'eth_requestAccounts'})
          .then(() => {
            this.connected = true // If users successfully connected their wallet
          })
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
