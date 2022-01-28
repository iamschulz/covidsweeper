<template>
	<span>
		Minesweeper, aber mit Sars-CoV-2. Die Schwierigkeit steigt mit der
		Inzidenz. In Deutschland liegt sie derzeit bei {{ incidence }} (<a
			href="https://api.corona-zahlen.org/docs/"
			>Quelle</a
		>).
	</span>
</template>

<script setup lang="ts">
defineProps({
	incidence: {
		type: Number,
		required: true,
	},
});
</script>
