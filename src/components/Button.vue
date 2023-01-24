<template>
	<button @click="onClick">{{ label }}</button>
</template>

<script>
export default {
	name: "Button",
	props: {
		label: {type: String},
		onClick: {type: Function}
	}
}
</script>

<style scoped>
button {
	background-color: var(--color-text);
	color: white;
	padding: 1em;
	border-radius: 8px;
	border: none;
	font-weight: bold;
	font-size: 18px;
}

</style>