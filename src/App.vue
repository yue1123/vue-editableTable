<script lang="ts" setup>
	import { ref } from 'vue'
	const tableSize = [5, 4]
	const rows = ref(
		Array.apply(null, { length: tableSize[0] }).map((_, i) =>
			Array.apply(null, { length: tableSize[1] }).map((_, j) => {
				return { text: i * tableSize[1] + j + 1, class: [] }
			})
		)
	)

	function onKeyupHandler(row, y, x, e) {
		console.log(row, x, y, e)
		const text = e.target.innerText
		// data.value.rows[y][x] = text
		// 删除
		// if (e.key === 'Backspace' && !text.length) {
		// 	// data.value.rows[y].splice(x, 1)
		// }
	}
	function addRows(y) {
		// data.value.rows.splice(y + 1, 0, ['', ''])
	}
</script>
<template>
	<div class="table">
		<div class="table-rows" v-for="row in rows">
			<div class="table-cell" v-for="cell in row" contenteditable="true">
				{{ cell.text }}
			</div>
			<div class="resize-cell"></div>
		</div>
	</div>
	<pre>

    <code>
  {{ JSON.stringify(rows,null,4) }}      
    </code>
  </pre>
</template>

<style lang="less">
	.table {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
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
			.resize-x {
				width: 100%;
				height: 3px;
				background: #f90;
				position: absolute;
			}
			.table-cell {
				&:first-child {
					border-left: 1px solid #f1f1f1;
				}
				display: flex;
				border-right: 1px solid #f1f1f1;
				flex-direction: column;
				width: 100%;
				height: 100%;
				padding: 7px 15px;
				cursor: edit;
				&:hover {
					background: #e2e2e2;
				}
			}
		}
	}
</style>
