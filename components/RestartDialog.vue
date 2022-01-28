<template>
	<dialog class="restartDialog" :open="open || null">
		<p>
			Du hast {{ points }} Menschen geimpft und dabei
			{{ points - falseFlags }} Infektionen verhindert. Den Rest hat
			Corona durchseucht.
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
});

const restartEmit = defineEmits(["restart"]);
</script>

<style lang="scss" scoped>
.restartDialog {
	top: 0;
	bottom: 0;
}

button {
	display: block;
	margin: auto;
}
</style>
