<template>
<div>
<div @show-popup="showPopup()"></div>
<transition name="fade">
<div id="popup" v-if="show">
	<div id="top">
		<div id="exit" @click="show = false">CLOSE</div>
	</div>
	<div id="msg">
		<h1>BINGO!</h1>
		<h6>Bingo Count: {{bingoCount}}</h6>
	</div>
</div>
</transition>
</div>
</template>

<style lang="scss" scoped>
#popup {
	position: absolute;
	display: flex;
	flex-direction: column;
	align-items: center;
	width: 80%;
	height: 50%;
	background: white;
	border: 2px solid black;
	z-index: 1;
	box-shadow: 0px 0px 70px 0px black;

	#top {
		position: relative;
		display: flex;
		width: 100%;
		height: 50px;

		#exit {
			position: absolute;
			right: 0px;
			display: flex;
			align-items: center;
			justify-content: center;
			width: 50px;
			height: 100%;
			background: grey;
			user-select: none;
		}

		#exit:hover {
			background: red;
		}
	}

	#msg {
		h1, h6 {
			padding: 0px;
			margin: 0px;
			text-align: center;
		}
	}
}
</style>

<script>
export default {
	name: "POPUP",
	methods: {
		showPopup(val) {
			console.log('val:', val)
			this.show = val
		}
	},
	data() {
		return {
			show: false
		}
	}
}
</script>