<template>
	<table>
		<thead>
			<tr>
				<th v-for="(row, index) in data.head" contenteditable="true">
					{{ row }}
				</th>
			</tr>
			<tr v-for="(row, y) in data.rows" :key="y">
				<td
					contenteditable="true"
					@keydown="($event) => onKeyupHandler(cell, y, x, $event)"
					:key="x"
					v-for="(cell, x) in row"
				>
					{{ cell }}
				</td>
				<button @click="addRows(y)">+</button>
			</tr>
		</thead>
	</table>
	<pre>

    <code>
  {{ JSON.stringify(data,null,4) }}      
    </code>
  </pre>
</template>

<script lang="ts" setup>
	import { ref } from 'vue'
	const data = ref({
		head: ['姓名', '年龄'],
		rows: [
			['111', '2'],
			['1', '21'],
			['11', '211'],
			['111', '2112'],
			[1, '211222']
		]
	})
	function onKeyupHandler(row, y, x, e) {
		console.log(row, x, y, e)
		const text = e.target.innerText
		data.value.rows[y][x] = text
		// 删除
		// if (e.key === 'Backspace' && !text.length) {
		// 	// data.value.rows[y].splice(x, 1)
		// }
	}
	function addRows(y) {
		data.value.rows.splice(y + 1, 0, ['', ''])
	}
</script>

<style>
	table {
		border-collapse: collapse;
	}
	table,
	th,
	td {
		border: 1px solid #000;
	}
	th,
	td {
		padding: 5px 10px;
	}
	tr {
		position: relative;
	}
	tr:hover {
		border-bottom: 3px solid #f90;
	}
	tr:hover button {
		display: inline-block;
	}
	code {
		width: 100%;
		display: block;
		text-align: left;
	}
	button {
		display: none;
		position: absolute;
		background: none;
		border: none;
		outline: none;
		padding: 4px;
		bottom: -50%;
		background: #f1f1f1;
	}
</style>
