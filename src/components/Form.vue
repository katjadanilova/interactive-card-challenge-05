<script lang="ts">
import Button from "./Button.vue"
import CustomInput from "./CustomInput.vue";
import {defineComponent} from "vue";

export default defineComponent({
	name: "Form",
	components: {
		CustomInput,
		Button
	},
	data() {
		return {
			showFormError: false,
			formSubmitted: false,
			cc:{
				nameValue: "",
				cardNumber: "",
				expDateMM: "",
				expDateYY: "",
				CVC: "",
			},
		}
	},
	methods: {
		handleSubmit() {
			this.showFormError = true;
			if (!this.formError)
				this.formSubmitted = true;
		},

	},
	computed: {
		formError(): string | null {
			if (this.cc.nameValue.length < 3)
				return "Name is too short"
			return null;
		}
	}
})

</script>

<template>
	<div class="container">
		<form @submit.prevent="handleSubmit" v-if="!formSubmitted">

			<CustomInput v-model="cc.nameValue" label="Cardholder Name"
						 placeholder="e.g. Jane Appleseed" kind="text"
						 :force-show-error="showFormError"/>

			<CustomInput kind="number" v-model="cc.cardNumber" label="Card Number"
						 placeholder="e.g. 1234 56789 9123 0000"
						 :force-show-error="showFormError"/>

			<div class="back">
				<div class="expiration">
					<CustomInput kind="number" v-model="cc.expDateMM" label="Exp.Date(MM/YY)" placeholder="MM"
								 :force-show-error="showFormError"/>
					<CustomInput kind="number" v-model="cc.expDateYY" placeholder="YY" :force-show-error="showFormError"/>
				</div>
				<CustomInput kind="number" v-model="cc.CVC" label="CVC" placeholder="e.g. 123"
							 :force-show-error="showFormError"/>
			</div>

			<div class="form-error" v-if="showFormError && formError">
				{{ formError }}
			</div>

			<Button/>


		</form>

		<div v-if="formSubmitted">
			Well done!
		</div>

	</div>
</template>


<style scoped>
.container {
	width: 400px;

}

.form-error {
	text-align: center;
	color: #c50000;
}

form {
	display: flex;
	flex-direction: column;
	gap: 1em;
}

.back {
	display: grid;
	grid-template-columns: 200px 200px;
	align-items: start;
}

.expiration {
	display: grid;
	grid-template-columns: 85px 85px;
	align-items: end;
	gap: 0.5em;
}
</style>
