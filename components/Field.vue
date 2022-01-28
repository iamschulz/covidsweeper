<template>
	<button
		class="field"
		:data-mine="isRevealed && isMine"
		:data-flagged="isFlagged"
		:data-revealed="isRevealed"
		:data-exploded="isExploded"
		:disabled="isRevealed"
		@click="(e) => emit('click', { e, index })"
		@contextmenu="(e) => !isFlagged && emit('rclick', { e, index })"
	>
		<span v-if="isMine && isFlagged && isRevealed">💉</span>
		<span v-else-if="isMine && isRevealed">🦠</span>
		<span v-else-if="isFlagged">💉</span>
		<span v-else-if="isRevealed" :data-value="value">{{ value }}</span>
	</button>
</template>

<script setup lang="ts">
const props = defineProps({
	index: {
		type: Number,
		required: true,
	},
	value: {
		type: Number,
		default: 0,
	},
	isMine: {
		type: Boolean,
		default: false,
	},
	isFlagged: {
		type: Boolean,
		default: false,
	},
	isRevealed: {
		type: Boolean,
		default: false,
	},
	isExploded: {
		type: Boolean,
		default: false,
	},
});

const emit = defineEmits(["click", "rclick"]);
</script>

<style lang="scss" scoped>
.field {
	cursor: pointer;
	border: 0;
	width: 2rem;
	aspect-ratio: 1;
	background-color: rgb(240, 240, 240);
	overflow: hidden;
	display: flex;
	place-items: center;
}

span {
	font-size: 1rem;
	font-weight: bold;
}

[data-value="0"] {
	color: transparent;
	background: transparent;
}

[data-value="1"] {
	color: blue;
}

[data-value="2"] {
	color: green;
}

[data-value="3"] {
	color: red;
}

[data-value="4"] {
	color: darkblue;
}

[data-value="5"] {
	color: maroon;
}

[data-value="6"] {
	color: slateblue;
}

[data-value="7"] {
	color: black;
}

[data-value="8"] {
	color: grey;
}

[data-revealed="true"][data-mine="true"] {
	background-color: rgb(255, 160, 165);
}

[data-revealed="true"][data-mine="true"][data-flagged="true"] {
	background-color: rgb(160, 255, 203);
}

[data-flagged="true"] {
	background-color: rgb(255, 160, 165);
}

[data-revealed="true"][data-mine="false"][data-flagged="true"] {
	background-color: rgb(252, 252, 172);
}

[data-revealed="true"][data-mine="true"][data-exploded="true"] {
	background-color: rgb(255, 0, 13);
}

[disabled] {
	color: rgba(0, 0, 0, 1);
	background-color: rgb(245, 245, 245);
}
</style>
