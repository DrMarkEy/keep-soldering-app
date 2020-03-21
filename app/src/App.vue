<template>
  <div id="app">    
  {{state.timeRemaining}}
	<button @click="test">Connect!</button>
  </div>
</template>

<script>


//----- STATUS ------
// Time Remaining
// #Strikes
// Serial #
// List of Modules
// Solved Modules

//------ CONFIGURATION ------
// Time
// Tick-Sound

//------> Einzelne Module


import uuids from './uuids';

export default {
  name: 'App',
  
  data: function() {
	return {
	  state: {
	    state: -1, //-1: Not connected, 0: inactive, 1: running, 2: paused, 3: solved, 4: exploded
	    timeRemaining: -1,	  
	    strikes: 0,
	    battery: -1,
	    charging: 0,
	    moduleCount: 0,
	    solvedCount: 0,
		serial: '',
	  },
	  
	  configuration: {
	    timeTotal: 0,
		tickSound: undefined
	  }
	}
  },
  
  methods: {
	test: async function() {
	  var thi = this;
	
	  // Step 1: Scan for a device with the general service
	  var device = await navigator.bluetooth.requestDevice({
		filters: [{ services: [uuids.services.state] }]
	  });
	 
	  var server = await device.gatt.connect();
	  var service = await server.getPrimaryService(uuids.services.state);
	  
	  var characteristic = await service.getCharacteristic(uuids.characteristics.state.state);
	  characteristic.startNotifications();
	  characteristic.addEventListener('characteristicvaluechanged', function(event) {
		thi.state.timeRemaining = event.target.value.getUint8(0)+256*event.target.value.getUint8(1);
	  });	  
	  
	  var val = await characteristic.readValue();
	  
	  
	  let decoder = new TextDecoder('utf-8');
      console.log('> Characteristic User Description: ' + decoder.decode(val));
	  
	  
	  
	  //SET LED
	  /*var LEDcharacteristic = await service.getCharacteristic("7961cceb-a950-4dc9-8f38-d8ed8931fce6");	  
	  
	  var str = "1";
	  let buffer = new ArrayBuffer(1);
      let dataView = new DataView(buffer);      
      dataView.setUint8(0, str.charAt(0).charCodeAt());      
	  
	  var val = await LEDcharacteristic.writeValue(buffer);*/
	  //console.log('value', val);
	  /*
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
