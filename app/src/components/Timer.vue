<template>
	<div>
		<seven-segment class="segmented-number" :value="m1" :rounded="false" :segment-width="25" :segment-height="5" on-color="#f00" off-color="transparent"/>
		<seven-segment class="segmented-number" :value="m2" :rounded="false" :segment-width="25" :segment-height="5" on-color="#f00" off-color="transparent"/>
		<span class="colon">:</span>
		<seven-segment class="segmented-number" :value="s1" :rounded="false" :segment-width="25" :segment-height="5" on-color="#f00" off-color="transparent"/>
		<seven-segment class="segmented-number" :value="s2" :rounded="false" :segment-width="25" :segment-height="5" on-color="#f00" off-color="transparent"/>
	</div>
</template>
<script>
import SevenSegmentDisplay from 'vue-seven-segment-display';

export default {
  name: 'timer',
  
  components: { "seven-segment": SevenSegmentDisplay },  
  
  props: ['value'],
  
  computed: {
    minutes: function()
	{
	  return pad(Math.floor(this.value / 60), 2);
	},
	
	seconds: function()
	{
	  return pad(this.value % 60, 2);
	},
  
	m1: function(){
	  return this.minutes.substr(0, 1);
	},
	
	m2: function(){
	  return this.minutes.substr(1);
	},
	
	s1: function(){
	  return this.seconds.substr(0, 1);
	},
	
	s2: function(){
	  return this.seconds.substr(1);
	},
  }
}

function pad(num, size) {
    var s = num+"";
    while (s.length < size) s = "0" + s;
    return s;
}
</script>
<style>
.segmented-number
{
	padding-right: 4px;
	padding-left: 4px;
}

.colon
{
	display: inline-block;
	color: red;
	font-size: 60pt;	
	transform: translate(0px, -8px);
}

</style>