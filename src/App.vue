<template>
<main ref="main">
	<popup />
	<board />
	<button id="reset" @click="reset">Reset Board and Reload Page</button>
</main>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Rubik:400,700');

html, body {
	position: relative;
	display: block;
	width: 100vw;
	height: 100vh;
	margin: 0;
	font-family: 'Rubik', sans-serif;
	background: orange;
	overflow: auto;
}

body * {
	transition: all 0.3s ease-in-out;
	user-select: none;
}

main {
	position: absolute;
	display: block;
	background: url("assets/bingobg.jpg");
	background-size: cover;
	background-position: center;
	z-index: 1;

	#reset {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
		background: #FACDCD;
		color: white;
		width: 100%;
		height: 60px;
		cursor: pointer;
	}

	#reset:hover {
		background: #FF9E9E;
	}
}
</style>

<script>
import {serverBus} from '@/main'
import popup from '@/components/popup'
import board from '@/components/board'

export default {
 name: 'CONTAINER',
 mounted() {
 	serverBus.$on('popup', val => {
 		if(val.show)
			document.body.style.overflow = "hidden"
		else
			document.body.style.overflow = "auto"
	})
 },
 methods: {
 	reset() {
 		serverBus.$emit('resetBoard', true)
 	}
 },
 components: {
  popup,
  board
 }
}
</script>
