<template>
	<dialog v-if="open" class="restartDialog" open>
		<h2 v-if="winner">💉 Glückwunsch!</h2>
		<h2 v-else>🦠 Das konnte niemand vorhersehen.</h2>
		<p>
			Du hast {{ points }} Menschen geimpft und dabei
			{{ points - falseFlags }} Infektionen verhindert.
			<span v-if="!winner">Den Rest hat Corona durchseucht.</span>
		</p>
		<button @click="() => restartEmit('restart')">Neuer versuch</button>
	</dialog>
</template>

<script setup lang="ts">
defineProps({
	open: {
		type: Boolean,
		default: false,
	},
	points: {
		type: Number,
		default: 0,
	},
	falseFlags: {
		type: Number,
		default: 0,
	},
	winner: {
		type: Boolean,
		default: false,
	},
});

const restartEmit = defineEmits(["restart"]);
</script>

<style lang="scss" scoped>
.restartDialog {
	position: absolute;
	top: 0;
	bottom: 0;
	display: block;
	width: fit-content;
	height: fit-content;
	margin: auto;
	background: white;
	padding: 1rem;
	border: 3px solid black;
}

h2 {
	margin: auto;
	text-align: center;
}

button {
	display: block;
	margin: auto;
}
</style>
