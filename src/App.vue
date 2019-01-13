<template>
<main ref="main">
 <popup />
 <board />
</main>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Rubik:400,700');

body {
 margin: 0;
 font-family: 'Rubik', sans-serif;
 overflow: auto;
}

body * {
 transition: all 0.3s ease-in-out;
}

main {
 position: relative;
 background: #5E5E5E;
 overflow: none;
 z-index: 1;
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
 components: {
  popup,
  board
 }
}
</script>
