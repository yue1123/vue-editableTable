<script lang="ts" setup>
	import { ref, onMounted } from 'vue'
	const tableSize = [5, 4]
	const table = ref()
	const width = ref(0)
	const preWidth = ref(0)
	const itemData = {
		text: '',
		class: []
	}
	const widths = ref<number[]>(genArrayLength(tableSize[1], 0))
	const rows = ref(genArrayLength(tableSize[0], genArrayLength(tableSize[1], itemData)))
	onMounted(() => {
		width.value = table.value.offsetWidth
		preWidth.value = width.value / tableSize[1]
	})
	function deepClone(obj) {
		return JSON.parse(JSON.stringify(obj))
	}
	function genArrayLength(length, fill) {
		return Array.apply(null, { length: length }).map(() => {
			return deepClone(fill)
		})
	}
	function onKeyupHandler(y, x, e) {
		const text = e.target.innerText
		rows.value[y][x].text = text
	}
	function handleAddRows(rowIndex: number) {
		rows.value.splice(rowIndex + 1, 0, genArrayLength(tableSize[1], deepClone(itemData)))
	}

	// resize
	let startPoint: { x: number } = null
	let isMouseDown = false
	let currentIndex = undefined
	function handleResizeStart(event: MouseEvent, index): void {
		// console.log(event)
		isMouseDown = true
		currentIndex = index
		startPoint = {
			x: event.clientX - (event.target as any).offsetLeft
		}
    console.log((event.target as any).offsetLeft);
	}
	function handleResizeEnd(event): void {
		isMouseDown = false
		startPoint = null
		console.log('end', event)
	}
	function handleResize(event: MouseEvent) {
		// console.log('1111', isMouseDown, startPoint, isMouseDown && startPoint)
		if (!isMouseDown && !startPoint) return
		const { clientX } = event
		// console.log(clientX - startPoint.x)
		let resizeValue = clientX - startPoint.x
		if (resizeValue < 0) {
			widths.value[currentIndex] -= resizeValue
			widths.value[currentIndex + 1] += resizeValue
		} else {
			widths.value[currentIndex] += resizeValue
			widths.value[currentIndex - 1] -= resizeValue
			// resizeValue = Math.floor(resizeValue / preWidth.value)
		}
		console.log(widths.value)
		console.log('currentIndex', currentIndex)
	}
</script>
<template>
	<div
		class="table"
		ref="table"
		@mousemove="($event) => handleResize($event)"
		@mouseup="handleResizeEnd"
		@mouseout.self="handleResizeEnd"
	>
		<div class="table-rows" v-for="(row, rowIndex) in rows">
			<div
				class="table-cell"
				v-for="(cell, cellIndex) in row"
				:style="{ width: `${preWidth}px` }"
				@keyup="($event) => onKeyupHandler(rowIndex, cellIndex, $event)"
				contenteditable="true"
			>
				{{ cell.text }}
			</div>
			<div class="add-rows" @click="() => handleAddRows(rowIndex)">
				<div></div>
			</div>
			<!-- <button class="select-rows"></button> -->
		</div>
		<div
			v-for="(_, index) in tableSize[1]"
			:style="{ left: `${preWidth * (index + 1) + widths[index]}px` }"
			class="resize-bar"
			@mousedown="($event) => handleResizeStart($event, index)"
		>
			<div class="table-cell-line"></div>
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
		width: 50%;
		height: 100%;
		position: relative;
		* {
			box-sizing: border-box;
		}
		.table-rows {
			&:first-child {
				border-top: 1px solid #ccc;
			}
			border-bottom: 1px solid #ccc;
			display: flex;
			flex-direction: row;
			width: 100%;
			height: 100%;
			position: relative;
			.table-cell {
				&:first-child {
					border-left: 1px solid #ccc;
				}
				text-align: left;
				width: 100%;
				height: 100%;
				padding: 5px 10px;
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
				cursor: pointer;
				&:hover div {
					opacity: 1;
				}
				div {
					display: inline-block;
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
		.resize-bar {
			position: absolute;
			height: 100%;
			width: 10px;
			transition: background 0.4s;
			background: transparent;
			cursor: col-resize;
			transform: translateX(-50%);
			&:hover {
				background: #f90;
				.table-cell-line {
					opacity: 0;
				}
			}
			.table-cell-line {
				height: 100%;
				width: 1px;
				background: #ccc;
				position: relative;
				margin-left: 5px;
			}
		}
	}
</style>
