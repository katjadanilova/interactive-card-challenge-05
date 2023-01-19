<script lang="ts">
import Button from "./Button.vue"
import CustomInput from "./CustomInput.vue";
import {defineComponent} from "vue";
import FormSuccess from "@/components/FormSuccess.vue";

function isInMonthlyRange(str: string): boolean {
	const pattern = /^(0[1-9]|1[0-2])$/;
	return pattern.test(str);
}

export default defineComponent({
	name: "Form",
	components: {
		FormSuccess,
		CustomInput,
		Button,
	},
	data() {
		return {
			showFormError: false,
			formSubmitted: false,
			cc: {
				nameValue: "",
				cardNumber: "",
				expDateMM: "",
				expDateYY: "",
				CVC: "",
			},
		}
	},
	watch: {
		cc: {
			handler(newValue) {
				this.$emit('sendData', newValue)
			},
			immediate: true,
			deep: true
		}
	},
	methods: {
		handleSubmit() {
			this.showFormError = true;
			if (!this.formError.length)
				this.formSubmitted = true;
		},
		toContinue() {
			this.formSubmitted = false;
			this.showFormError = false;
			for (const prop in this.cc) {
				this.cc[prop] = ""
			}
		}

	},
	computed: {
		formError(): string[] {
			const errorArray = []

			const date = new Date();
			const year = date.getFullYear().toString().slice(-2);

			if (this.cc.expDateYY < year) {
				errorArray.push("Enter the expiration year in YY format, such as '25' for the year 2025.")
			}
			if (!isInMonthlyRange(this.cc.expDateMM)) {
				errorArray.push("Enter the expiration month in MM format, such as '01' for January.")
			}
			return errorArray;
		}
	},
	emits: ["sendData"]
})

</script>

<template>
	<div class="form-container" >
		<form @submit.prevent="handleSubmit" v-if="!formSubmitted">

			<CustomInput v-model="cc.nameValue" label="Cardholder Name"
						 placeholder="e.g. Jane Appleseed" kind="text"
						 :force-show-error="showFormError"/>

			<CustomInput kind="number" v-model="cc.cardNumber" label="Card Number"
						 placeholder="e.g. 1234 56789 9123 0000"
						 :force-show-error="showFormError"/>

			<div class="expiration-details">
				<div class="expiration">
					<p>Exp.Date(MM/YY)</p>
					<div class="expiration-dates">
						<CustomInput kind="date" v-model="cc.expDateMM" placeholder="MM"
									 :force-show-error="showFormError"/>
						<CustomInput kind="date" v-model="cc.expDateYY" placeholder="YY"
									 :force-show-error="showFormError"/>
					</div>
				</div>
				<CustomInput kind="cvc" v-model="cc.CVC" label="CVC" placeholder="e.g. 123"
							 :force-show-error="showFormError"/>
			</div>

			<div class="form-error" v-if="showFormError && formError.length">
				<p v-for="error in formError">{{ error }}</p>
			</div>

			<Button label="Confirm"/>
		</form>

		<div v-if="formSubmitted" class="form-success">
			<FormSuccess/>
			<Button :onClick="toContinue" label="Continue"/>
		</div>

	</div>
</template>


<style scoped>
@media screen and (max-width: 950px) {
	.form-container {
		padding: 1em;
		width: 100% !important;
	}
}

.form-container {
	width: 400px;
	padding: 1em
}

.form-error {
	font-size: 12px;
	color: #c50000;
	border: solid 1px #c50000;
	padding: 1em;
	background-color: #fff3f3;
	border-radius: 8px;
}

form {
	display: flex;
	flex-direction: column;
	gap: 1em;
}

.expiration-details {
	display: grid;
	grid-template-columns: 1fr 1fr;
	align-items: start;
}

.expiration {
	display: flex;
	flex-direction: column;
	align-items: start;
	padding-right: 1em;
}

.expiration-dates {
	display: flex;
	gap: 0.5em;
}

.form-success {
	display: flex;
	flex-direction: column;
	align-items: stretch;
}


</style>
