<script lang="ts" setup>
	import { ref } from 'vue'
	const tableSize = [5, 4]
	const rows = ref(
		Array.apply(null, { length: tableSize[0] }).map((_, i) =>
			Array.apply(null, { length: tableSize[1] }).map((_, j) => {
				return { text: '', class: [] }
			})
		)
	)

	function onKeyupHandler(y, x, e) {
		const text = e.target.innerText
		rows.value[y][x].text = text
	}
	function handleAddRows(rowIndex: number) {
		rows.value.splice(
			rowIndex + 1,
			0,
			Array.apply(null, { length: tableSize[1] }).map((_, j) => {
				return { text: '', class: [] }
			})
		)
	}
</script>
<template>
	<div class="table">
		<div class="table-rows" v-for="(row, rowIndex) in rows">
			<div
				class="table-cell"
				v-for="(cell, cellIndex) in row"
				@keyup="($event) => onKeyupHandler(rowIndex, cellIndex, $event)"
				contenteditable="true"
			>
				{{ cell.text }}
			</div>
			<div class="add-rows">
				<button @click="() => handleAddRows(rowIndex)"></button>
			</div>
			<!-- <button class="select-rows"></button> -->
		</div>
	</div>
	<!-- <pre>

    <code>
  {{ JSON.stringify(rows,null,4) }}      
    </code>
  </pre> -->
</template>

<style lang="less">
	.table {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
		* {
			box-sizing: border-box;
		}
		.table-rows {
			&:first-child {
				border-top: 1px solid #f1f1f1;
			}
			border-bottom: 1px solid #f1f1f1;
			display: flex;
			flex-direction: row;
			width: 100%;
			height: 100%;
			position: relative;
			.table-cell {
				&:first-child {
					border-left: 1px solid #f1f1f1;
				}
				border-right: 1px solid #f1f1f1;
				flex-direction: column;
				width: 100%;
				height: 100%;
				padding: 7px 15px;
				cursor: edit;
			}
			.resize-x {
				width: 100%;
				height: 3px;
				background: #f90;
				position: absolute;
			}
			.add-rows {
				padding: 3px;
				position: absolute;
				left: 0;
				bottom: 0;
				transform: translate(-50%, 50%);
				z-index: 2;
				&:hover button {
					opacity: 1;
				}
				button {
					transition: opacity 0.4s;
					opacity: 0;
					padding: 0;
					width: 7px;
					height: 7px;
					border: none;
					outline: none;
					background: #f90;
					border-radius: 50%;
				}
			}
		}
	}
</style>
