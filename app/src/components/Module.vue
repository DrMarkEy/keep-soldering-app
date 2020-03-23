<template>
	<li class="module">
		<div class="icon" :style="{'background-position': sprites[type]}"></div>
		<span class="name">{{names[type]}}</span>
	</li>
</template>
<script>
var sprites = {
	1: '0px 0px',
	2: '-224px -960px',
	3: '-576px -1120px',
	4: '-224px -480px',
	5: '-544px -832px',
	6: '-320px -1120px',
	7: '-64px -576px',
	8: '-128px -608px',
	9: '-608px -192px',
	10: '-512px -1120px',
	11: '-512px -544px',
	12: '-0px -704px',
	13: '-608px -1088px',
	14: '-128px -160px',
	15: '-288px -480px',
};

var names = {
	1: 'Timer',
	2: 'Button',
	3: 'Kabel',
	4: 'Keypads',
	5: 'Simon Says',
	6: 'Who\'s on first',
	7: 'Memory',
	8: 'Morse',
	9: 'Komplizierte Kabel',
	10: 'Kabelsequenzen',
	11: 'Labyrinth',
	12: 'Passwort',
	13: 'Austretendes Gas',
	14: 'Kondensatorentladung',
	15: 'Drehknöpfe',
}


export default {
  name: 'module',
  
  computed: {
	sprites: function()
	{
		return sprites;
	},
	
	names: function()
	{
		return names;
	}
  },
  
  props: ['type']  
}

</script>
<style>
.module .icon
{	
	width: 32px;
	height: 32px;
	background-image: url('../assets/spritesheet.png');
	background-position:0px 0px;
	float: left;
	margin-bottom: 8px;
}

.module .name
{
	display: inline-block;
	padding-top: 6px;
}

.module
{
	margin-left: 00px;
	margin-right: 30px;
	clear: both;	
}

</style>