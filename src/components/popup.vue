<template>
<transition name="fade">
<div ref="popup" id="popup" v-if="show" @click.self="hide" tarnsition="fade">
	<div v-if="show" id="popup-inner">
		<div id="top">
			<div id="exit" @click="hide">CLOSE</div>
		</div>
		<div id="msg">
			<h1>BINGO!</h1>
			<h6>Bingo Count: {{bingoCount}}</h6>
		</div>
	</div>
</div>
</transition>
</template>

<style lang="scss" scoped>
.fade-enter, .fade-leave-to {
	opacity: 0;
}

.fade-enter #popup-inner, .fade-leave-to #popup-inner{
	transform: translateY(-20%);
}

.fade-enter-to {
	opacity: 1;
	transform: translateY(0%);
}

#popup {
	position: absolute;
	display: flex;
	align-items: center;
	justify-content: center;
	left: 0px;
	top: 0px;
	width: 100vw;
	height: 100vh;
	background: rgba(0, 0, 0, 0.6);
	z-index: 3;

	#popup-inner {
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 80%;
		height: 50%;
		background: white;
		border: 2px solid black;
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
}
</style>

<script>
import {serverBus} from '@/main'

export default {
	name: "POPUP",
	mounted() {
		serverBus.$on('popup', val => {
		   this.show = val.show
		   this.bingoCount = val.bingoCount
		})

		document.onkeydown = e => {
			if(this.show && (e.key =="Escape" || e.key == "Esc"))
				this.hide()
		}
	},
	methods: {
		hide() {
			serverBus.$emit('popup', {
				show: false,
				bingoCount: this.bingoCount
			})
		}
	},
	data() {
		return {
			show: false,
			bingoCount: 0
		}
	}
}
</script>