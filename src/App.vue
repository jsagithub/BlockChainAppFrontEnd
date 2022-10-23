<template>
  <div id="app">
    <button v-if="!connected" @click="connect">Connect wallet</button>
    <button v-if="connected" @click="callContract">Call contract</button>
    {{ contractResult }}
  </div>
</template>

<script>
import Web3 from 'web3'
import Location from './assets/Location.json'

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
    },

    callContract: function () {
      // method for calling the contract method
      let web3 = new Web3(window.ethereum)
      const networkId = 5777
      const networkData = Location.networks[networkId]
      if (networkData) {
        let contract = new web3.eth.Contract(Location.abi, networkData.address)
        contract.methods.createProduct("Austin Martin", "1000000000000000000")
          .call()

        const product = contract.methods.products(1).call()
        this.contractResult = 'Name: '+ product.name;

      } else {
        window.alert('Location contract not deployed to detected network.')
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
