<template>
	<div>
		<Header />
		<main class="content">
			<div class="info">
				<Info :incidence="incidence" />
				<Score :points="points" />
			</div>
			<Board
				v-if="!loading"
				:incidence="incidence"
				@score="points++"
				@unscore="points--"
				@falseflag="falseFlags++"
				@unfalseflag="falseFlags--"
				@gameover="onGameOver"
			/>
			<div class="loading" v-if="loading">
				<LoadingIcon />
			</div>
			<RestartDialog
				:open="gameover"
				:points="points"
				:falseFlags="falseFlags"
				:winner="win"
				@restart="onRestart"
			/>
		</main>
	</div>
</template>

<script setup lang="ts">
const incidence = ref(0);
const points = ref(0);
const falseFlags = ref(0);
const gameover = ref(false);
const win = ref(false);
const loading = ref(true);
const lastFetched = ref<number | null>(null);

const onRestart = () => {
	gameover.value = false;
	points.value = 0;
	falseFlags.value = 0;
	fetchData();
};

const onGameOver = (condition) => {
	win.value = condition === "win";
	gameover.value = true;
};

const fetchData = async () => {
	loading.value = true;

	if (lastFetched.value > new Date().getTime() - 6 * 60 * 60 * 1000) {
		window.setTimeout(() => {
			loading.value = false;
		}, 0);
		return;
	}

	const data = await fetch("https://api.corona-zahlen.org/germany").then(
		(response) => response.json()
	);
	incidence.value = Math.round(data.weekIncidence);
	lastFetched.value = new Date().getTime();
	window.setTimeout(() => {
		loading.value = false;
	}, 0);
};

onMounted(() => {
	fetchData();
});
</script>

<style lang="scss">
body {
	font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS",
		sans-serif;
}

.content {
	position: relative;
	display: grid;
	gap: 1rem;
	max-width: 80vw;
	margin: auto;
}

.info {
	display: flex;
	justify-content: space-between;
	align-items: center;
	gap: 1rem;
	margin: auto;
}

.loading {
	display: grid;
	place-items: center;
	min-height: 10rem;
}
</style>
