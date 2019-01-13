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
	flex-direction: column;
	background: url("../assets/bingobg.jpg");
	background-size: cover;
	background-position: center;
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
			min-width: 120px;
			width: 20vw;
			min-height: 120px;
			height: 20vh;
			background: rgba(0, 0, 0, 0.2);
			border: 1px solid white;
			text-align: center;
			font-size: 18px;
			user-select: none;
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

export default {
name: "BOARD",
methods: {
	bingoSelect(row, column) {
		if(column.checked && this.win)
			this.win = false

		column.checked = !column.checked
		this.checkWin(row, column)
	},
	checkWin(row, column) {
		let win = false

		if(!win)
			win = this.checkHorizontal(row)
		if(!win)
			win = this.checkVertical(row, column)
		if(!win)
			win = this.checkDiagonal(row, column)

		if(win){
			this.bingoCount++
			serverBus.$emit('popup', {
				show: true,
				bingoCount: this.bingoCount
			})
		}
	},
	checkHorizontal(row) {
		let checkCount = 0
		row.forEach(column=>{if(column.checked){checkCount++}})
		return checkCount === 5
	},
	checkVertical(row, column) {
		let checkCount = 0
		let col = row.indexOf(column)
		this.board.forEach( row => {if(row[col].checked){checkCount++}})
		return checkCount === 5
	},
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
data() {
	return {
		board: [
			[ // row 0
				{
					todo: "ENEMY ATTACKS FROM SPACE",
					checked: false
				},
				{
					todo: "WEAPONIZED WEATHER",
					checked: false
				},
				{
					todo: "GOOD GUYS WERE BAD ALL ALONG",
					checked: false
				},
				{
					todo: "ESCORT MISSION",
					checked: false
				},
				{
					todo: "SURVIVE UNTIL ALLIES ARRIVE",
					checked: false
				}
			],

			[ // row 1
				{
					todo: "WINGMAN DIES",
					checked: false
				},
				{
					todo: "PLAYER CHARACTER EXCLUDED FROM CINEMATICS",
					checked: false
				},
				{
					todo: "RAZGRIZ REFERENCE",
					checked: false
				},
				{
					todo: "ATTEMPTEED MURDER-SUICIDE",
					checked: false
				},
				{
					todo: "DECIEVING THE ENEMY",
					checked: false
				}
			],

			[ // row 2
				{
					todo: "DESTROY A SUPER WEAPON",
					checked: false
				},
				{
					todo: "ENEMIES WERE BAITED INTO WAR",
					checked: false
				},
				{
					todo: "TUNNEL RUN SEGMENT",
					checked: false
				},
				{
					todo: "\"DEAD\" CHARACTER RETURNS AT THE END",
					checked: false
				},
				{
					todo: "IT WAS BELKA",
					checked: false
				}
			],

			[ // row 3
				{
					todo: "TOKEN ROCK SONG",
					checked: false
				},
				{
					todo: "FRIENDLY POLITICIAN IS KIDNAPPED",
					checked: false
				},
				{
					todo: "NO-WEAPON MISSION",
					checked: false
				},
				{
					todo: "ENEMY ACE CAN GET SNIPED BY SPECIALS",
					checked: false
				},
				{
					todo: "DEFEND ENEMY DEFECTOR",
					checked: false
				}
			],

			[	// row 4
				{
					todo: "SHOOTING DOWN MISSILES",
					checked: false
				},
				{
					todo: "XENOPHOBIA",
					checked: false
				},
				{
					todo: "NUKES ARE INVOLVED",
					checked: false
				},
				{
					todo: "ASSISTING A GROUND ASSAULT",
					checked: false
				},
				{
					todo: "LITERALLY FLYING UNDER THE RADAR",
					checked: false
				}
			]
		],
		bingoCount: 0
	}
}
}
</script>