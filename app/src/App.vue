<template>
  <div id="app">    
    <timer :value="state.timeRemaining"/><br/>
	
	<button @click="connect">Connect!</button>
	<hr/>
	<button @click="pause">Pause</button>
	<button @click="resume">Resume</button>
	<!--<button>Resume</button>-->
  </div>
</template>

<script>
import Timer from './components/Timer.vue';

//Hauptmenü:
//Spielen
	//Freies Spiel
		//-> Spiel mit den verbauten Modulen
	//Kampagne	
		//-> Zu verbauende Module +Zeit werden von App vorgegeben
	//Anleitung öffnen
		//-> PDF wird geöffnet
	//Bombenleger-Modus
		//-> Play/Pause und Timer verändern, Events auslösen etc. für Geburtstagsfeiern
//Konfigurieren
	//Allgemeine Eigenschaften
		//-> Lautstärke, Tick-Sound, Sprache, Vibration...
	//Modul-Settings		
	

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
  
  components: { Timer },  
  
  data: function() {
	return {
	  state: {
	    state: -1, //-1: Not connected, 0: inactive, 1: running, 2: paused, 3: solved, 4: exploded
	    timeRemaining: 0,	  
	    strikes: 0,
	    batteryLevel: -1,
	    charging: 0,
	    moduleCount: 0,
	    solvedCount: 0,
		serial: '',
	  },
	  
	  configuration: {
	    timeTotal: 0,
		tickSound: undefined
	  },
	  
	  commandCharacteristic: undefined
	}
  },
  
  methods: {
    pause: async function() {
	  let commandValue = new Uint8Array([0x02]);
	  this.commandCharacteristic.writeValue(commandValue);
	},
	
	resume: async function() {
	  let commandValue = new Uint8Array([0x03]);
	  this.commandCharacteristic.writeValue(commandValue);
	},
  
	connect: async function() {
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
	    
		  var buffer = event.target.value;
		  thi.state.state = buffer.getUint8(0);
		  thi.state.timeRemaining = event.target.value.getUint8(1)+256*event.target.value.getUint8(2);
		  thi.state.strikes = event.target.value.getUint8(3);
		  thi.state.batteryLevel = event.target.value.getUint8(4);
		  thi.state.charging = event.target.value.getUint8(5);
	  });	  
	 
	  this.commandCharacteristic = await service.getCharacteristic(uuids.characteristics.state.command);	  
	  
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
