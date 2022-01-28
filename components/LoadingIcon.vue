<template>
	<div class="loadingIcon"></div>
</template>
<style lang="scss" scoped>
@keyframes spin {
	0% {
		transform: rotate(0deg);
	}
	100% {
		transform: rotate(360deg);
	}
}

.loadingIcon {
	width: 5rem;
	height: 5rem;
	border-radius: 50%;
	border: 10px solid #000;
	border-top-color: #fff;
	animation: spin 1s linear infinite;
}
</style>
