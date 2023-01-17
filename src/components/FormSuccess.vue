<template>
	<div>
		<img src="../assets/icon-complete.svg" alt="Success!">
		<h1>Thank you!</h1>
		<p>We've added your card details.</p>

	</div>
</template>

<script lang="ts">
import Button from "./Button.vue"
import {defineComponent} from 'vue'

export default defineComponent({
	name: "FormSuccess",
	components: {
		Button
	}

})
</script>

<style scoped>
div {
	display: flex;
	flex-direction: column;
	align-items: center;
	gap: 1em;
	margin-bottom: 3em;
}

h1 {
	margin-top: 1em;
	text-transform: uppercase;
}

p{
	color: var(--vt-c-text-light-2);
}


</style>