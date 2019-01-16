<template>
<div id="board">
	<div v-for="row in board" :key="board[row]" class="row">
		<div v-for="column in row" :key="row[column]" :class="[ column.checked ? 'column checked' : 'column' ]" @click="bingoSelect(row, column)">
			{{ column.todo }}
		</div>
	</div>
</div>
</template>

<style lang="scss" scoped>
#board {
	position: relative;
	display: flex;
	width: 100%;
	height: 100%;
	flex-direction: column;
	z-index: 2;

	.row {
		position: relative;
		display: flex;
		flex-direction: row;

		.column {
			display: flex;
			align-items: center;
			justify-content: center;
			box-sizing: border-box;
			padding: 10px;
			min-width: 150px;
			width: 20vw;
			min-height: 150px;
			height: 20vh;
			background: rgba(0, 0, 0, 0.2);
			border: 1px solid rgba(255, 255, 255, 0.2);
			text-align: center;
			font-size: 20px;
			cursor: pointer;
			color: white;
			-webkit-text-stroke: 1px black;
			font-weight: 700;
		}

		.column:hover {
			background: rgba(0, 0, 0, 0.6);
		}

		.checked {
			background: rgba(26, 255, 0, 0.28)
		}

		.checked:hover {
			background: rgba(255, 84, 0, 0.45);
		}
	}
}
</style>

<script>
import {serverBus} from '@/main'
import Cookie from 'js-cookie'

export default {
name: "BOARD",

components: {
	Cookie,
},

methods: {
	bingoSelect(row, column) {
		column.checked = !column.checked
		this.setCheckCookie(row, column)
		this.checkWin(row, column)
	},

	setCheckCookie(row, column) {
		let selected = []

		if(Cookie.get('bingo_selected'))Cookie.get('bingo_selected').split('|').forEach(index=>{selected.push(index)})

		if(column.checked){
			selected.push(this.board.indexOf(row) + '-' + row.indexOf(column))
		}else{
			let index = selected.indexOf(this.board.indexOf(row) + '-' + row.indexOf(column))
			if(index > -1)selected.splice(index, 1)
		}

		let d = new Date(); d.setTime(d.getTime() + 60000)
		Cookie.set('bingo_selected', selected.join('|'), {expires: d })
	},

	checkWin(row, column) {
		let win = false

		if(!win)win = this.checkHorizontal(row)
		if(!win)win = this.checkVertical(row, column)
		if(!win)win = this.checkDiagonal(row, column)

		if(win){serverBus.$emit('popup', true)}
	},

	checkHorizontal(row) {
		let checkCount = 0
		row.forEach(column=>{if(column.checked)checkCount++})
		return checkCount === 5
	},

	checkVertical(row, column) {
		let checkCount = 0
		let colIndex = row.indexOf(column)
		this.board.forEach(row=>{if(row[colIndex].checked)checkCount++})
		return checkCount === 5
	},

	// refactor this mess
	checkDiagonal(row, column) {
		let checkCount1 = 0
		let checkCount2 = 0
		let coord = [this.board.indexOf(row), row.indexOf(column)]
		let tempCoord = [...coord]
		let diag1 = []
		let diag2 = []

		diag1.push(this.board[tempCoord[0]][tempCoord[1]])
		diag2.push(this.board[tempCoord[0]][tempCoord[1]])

		for(let x = 0; x < this.board.length; x++) {
			tempCoord[0] += 1
			tempCoord[1] += 1
			if((tempCoord[0] >= 0 && tempCoord[0] < this.board.length) && (tempCoord[1] >= 0 && tempCoord[1] < this.board.length))
				diag1.push(this.board[tempCoord[0]][tempCoord[1]])
		}

		tempCoord = [...coord]

		for(let x = 0; x < this.board.length; x++) {
			tempCoord[0] -= 1
			tempCoord[1] -= 1
			if((tempCoord[0] >= 0 && tempCoord[0] < this.board.length) && (tempCoord[1] >= 0 && tempCoord[1] < this.board.length))
				diag1.push(this.board[tempCoord[0]][tempCoord[1]])
		}

		tempCoord = [...coord]

		for(let x = 0; x < this.board.length; x++) {
			tempCoord[0] += 1
			tempCoord[1] -= 1
			if((tempCoord[0] >= 0 && tempCoord[0] < this.board.length) && (tempCoord[1] >= 0 && tempCoord[1] < this.board.length))
				diag2.push(this.board[tempCoord[0]][tempCoord[1]])
		}

		tempCoord = [...coord]

		for(let x = 0; x < this.board.length; x++) {
			tempCoord[0] -= 1
			tempCoord[1] += 1
			if((tempCoord[0] >= 0 && tempCoord[0] < this.board.length) && (tempCoord[1] >= 0 && tempCoord[1] < this.board.length))
				diag2.push(this.board[tempCoord[0]][tempCoord[1]])
		}

		diag1.forEach( column => {if(column.checked){checkCount1++}})
		diag2.forEach( column => {if(column.checked){checkCount2++}})

		return (checkCount1 === 5 || checkCount2 === 5)
	}
},

mounted() {

	serverBus.$on('resetBoard', val => {if(val)Cookie.remove('bingo_board'); Cookie.remove('bingo_selected'); location.reload()})

	let list;
	if(Cookie.get('bingo_board'))list = Cookie.get('bingo_board').split('|')
	else{
		list = [
			"ENEMY ATTACKS FROM SPACE",
			"WEAPONIZED WEATHER",
			"GOOD GUYS WERE BAD ALL ALONG",
			"ESCORT MISSION",
			"SURVIVE UNTIL ALLIES ARRIVE",
			"WINGMAN DIES",
			"PLAYER CHARACTER EXCLUDED FROM CINEMATICS",
			"RAZGRIZ REFERENCE",
			"ATTEMPTEED MURDER-SUICIDE",
			"DECIEVING THE ENEMY",
			"DESTROY A SUPER WEAPON",
			"ENEMIES WERE BAITED INTO WAR",
			"TUNNEL RUN SEGMENT",
			"\"DEAD\" CHARACTER RETURNS AT THE END",
			"IT WAS BELKA",
			"TOKEN ROCK SONG",
			"FRIENDLY POLITICIAN IS KIDNAPPED",
			"NO-WEAPON MISSION",
			"ENEMY ACE CAN GET SNIPED BY SPECIALS",
			"DEFEND ENEMY DEFECTOR",
			"SHOOTING DOWN MISSILES",
			"XENOPHOBIA",
			"NUKES ARE INVOLVED",
			"ASSISTING A GROUND ASSAULT",
			"LITERALLY FLYING UNDER THE RADAR"
		]

		function shuffle(a) {for (let i = a.length - 1; i > 0; i--){const j = Math.floor(Math.random() * (i + 1)); [a[i], a[j]] = [a[j], a[i]]} return a;}
		shuffle(list)

		let d = new Date(); d.setTime(d.getTime() + 60000)
		Cookie.set('bingo_board', list.join('|'), {expires: d })
	}

	let count = 0;
	for(let i = 0; i < 5; i++){let temp = []; for(let j = 0; j < 5; j++){temp.push({todo: list[count], checked: false}); count++}this.board.push(temp)}

	if(Cookie.get('bingo_selected')){
		let indexes = Cookie.get('bingo_selected').split('|')
		for(let i = 0; i < indexes.length; i++){
			let index = indexes[i].split('-')
			this.board[index[0]][index[1]].checked=true
		}
	}
},
data() {
	return {
		board: []
	}
}
}
</script>