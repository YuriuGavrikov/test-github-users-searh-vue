<template>
	<button class="btn">
		<slot></slot>
	</button>
</template>

<script>
export default {
	name: "MainButton",
};
</script>

<style scoped>
.btn {
	width: 70px;
	padding: 10px;
	border: none;
	border-radius: 6px;
	background: #fff;
	color: grey;
	box-shadow: 5px 5px 5px -2px rgba(181, 189, 195, 0.5);
}
</style>
