<template>
	<span>
		💉&nbsp;Impfungen:&nbsp;<b>{{ points }}</b>
	</span>
</template>

<script setup lang="ts">
defineProps({
	points: {
		type: Number,
		default: 0,
	},
});
</script>
