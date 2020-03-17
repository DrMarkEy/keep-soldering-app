<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
	
	<button @click="test">TEST</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  
  methods: {
	test: async function() {
	  // Step 1: Scan for a device with 0xffe5 service
	  var device = await navigator.bluetooth.requestDevice({
		filters: [{ services: ["d9696362-1986-4ffd-8be4-ce7a8acada42"] }]
	  });
	 
	  var server = await device.gatt.connect();
	  var service = await server.getPrimaryService(0xffe5);
	  
	  console.log('service', service);
	  /*
	  .then(function(service) {
		// Step 4: get the Characteristic
		return service.getCharacteristic(0xffe9);
	  })
	  .then(function(characteristic) {
		// Step 5: Write to the characteristic
		var data = new Uint8Array([0xbb, 0x25, 0x05, 0x44]);
		return characteristic.writeValue(data);
	  })
	  .catch(function(error) {
		 // And of course: error handling!
		 console.error('Connection failed!', error);
	  });*/
	}
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
