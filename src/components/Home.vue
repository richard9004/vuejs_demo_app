<template>
	<div>
		<h1>Home Page</h1>
		<button v-on:click="welcome">Click Me</button>
	</div>
	
</template>

<script>
export default{
	name:'Home',
	props:{
		msg:String
	},
	methods:{
		welcome(){
			alert("Home Page");
		}
	}
}
	
</script>