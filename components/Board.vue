<template>
	<section class="board">
		<Field
			v-for="(field, index) in fields"
			:key="index"
			:index="field.index"
			:value="getFieldValue(field.index)"
			:isMine="field.isMine"
			:isFlagged="field.isFlagged"
			:isQuestioned="field.isQuestioned"
			:isRevealed="field.isRevealed"
			:isExploded="field.isExploded"
			@click="handleLeftClick"
			@rclick="handleRightClick"
		/>
	</section>
</template>

<script setup lang="ts">
const boardProps = defineProps({
	incidence: {
		type: Number,
		required: true,
	},
});

const boardEmit = defineEmits([
	"score",
	"unscore",
	"falseflag",
	"unfalseflag",
	"gameover",
]);

const width = 30;
const height = 30;

const difficulty = Math.log(boardProps.incidence * 3) * 10;
const minesPerc = Math.min((boardProps.incidence / 100_000) * difficulty, 1);
//console.log(minesPerc);

const generateMines = () => {
	const mines = [];
	while (mines.length < width * height * minesPerc) {
		const index = Math.floor(Math.random() * (width * height));
		if (!mines.includes(index)) {
			mines.push(index);
		}
	}
	return mines;
};
const mines = generateMines();

const fields = ref([]);
for (let i = 0; i < width * height; i++) {
	fields.value.push({
		index: i,
		isMine: mines.includes(i),
		isFlagged: false,
		isRevealed: false,
		isExploded: false,
	});
}

const getFieldValue = (index: number) => {
	const checkLeft = index % width !== 0;
	const checkRight = index % width !== width - 1;

	const tl = checkLeft ? fields.value[index - width - 1] : 0;
	const t = fields.value[index - width];
	const tr = checkRight ? fields.value[index - width + 1] : 0;
	const l = checkLeft ? fields.value[index - 1] : 0;
	const r = checkRight ? fields.value[index + 1] : 0;
	const bl = checkLeft ? fields.value[index + width - 1] : 0;
	const b = fields.value[index + width];
	const br = checkRight ? fields.value[index + width + 1] : 0;

	return (
		(tl?.isMine ? 1 : 0) +
		(t?.isMine ? 1 : 0) +
		(tr?.isMine ? 1 : 0) +
		(l?.isMine ? 1 : 0) +
		(r?.isMine ? 1 : 0) +
		(bl?.isMine ? 1 : 0) +
		(b?.isMine ? 1 : 0) +
		(br?.isMine ? 1 : 0)
	);
};

type Payload = {
	e: Event;
	index: number;
};

const revealArea = (index: number) => {
	const field = fields.value[index];
	if (!field || field.isRevealed || field.isFlagged) {
		return;
	}

	fields.value[index].isRevealed = true;

	const revealLeft = index % width !== 0;
	const revealRight = index % width !== width - 1;
	if (getFieldValue(index) === 0) {
		revealLeft && revealArea(index - width - 1); // top left
		revealArea(index - width); // top
		revealRight && revealArea(index - width + 1); // top right
		revealLeft && revealArea(index - 1); // left
		revealRight && revealArea(index + 1); // right
		revealLeft && revealArea(index + width - 1); // bottom left
		revealArea(index + width); // bottom
		revealRight && revealArea(index + width + 1); // bottom right
	}
};

const handleLeftClick = (payload: Payload) => {
	const { index } = payload;

	if (fields.value[index].isFlagged) {
		fields.value[index].isFlagged = false;
		boardEmit("unscore");
		if (!fields.value[index].isMine) {
			boardEmit("unfalseflag");
		}
		return;
	}

	if (fields.value[index].isMine) {
		fields.value[index].isRevealed = true;
		gameOver(index);
		return;
	}

	revealArea(index);
	fields.value[index].isRevealed = true;

	if (fields.value.every((field) => field.isRevealed || field.isFlagged)) {
		boardEmit("gameover", "win");
	}
};

const handleRightClick = (payload: Payload) => {
	const { e, index } = payload;
	e.preventDefault();

	fields.value[index].isFlagged = true;
	boardEmit("score");

	if (!fields.value[index].isMine) {
		boardEmit("falseflag");
	}

	if (fields.value.every((field) => field.isRevealed || field.isFlagged)) {
		boardEmit("gameover", "win");
	}
};

const gameOver = (index: number) => {
	fields.value[index].isExploded = true;
	fields.value.forEach((field) => {
		field.isRevealed = true;
	});
	boardEmit("gameover", "lose");
};
</script>

<style lang="scss" scoped>
.board {
	display: grid;
	grid-template-columns: repeat(v-bind(width), 1fr);
	grid-template-rows: repeat(v-bind(height), 1fr);
	gap: 1px;
	margin: auto;
}
</style>
