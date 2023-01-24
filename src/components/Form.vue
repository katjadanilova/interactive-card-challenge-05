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
			shakeEnabled: false,
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
		},
		enableShake() {
			if (this.formError.length) {
				this.shakeEnabled = true
				setTimeout(() => {
					this.shakeEnabled = false
				}, 1500)
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
			if (!this.cc.cardNumber || !this.cc.nameValue || !this.cc.CVC)
				errorArray.push("Fill in all the required fields.")

			return errorArray;
		}
	},
	emits: ["sendData"]
})

</script>

<template>
	<div class="form-container">
		<form @submit.prevent="handleSubmit" v-if="!formSubmitted">

			<CustomInput v-model="cc.nameValue" label="Cardholder Name"
						 placeholder="e.g. Jane Appleseed" kind="text"
						 :force-show-error="showFormError"/>

			<CustomInput kind="number" v-model="cc.cardNumber" label="Card Number"
						 placeholder="e.g. 1234 56789 9123 0000"
						 :force-show-error="showFormError"/>

			<div class="expiration-details">
				<div class="expiration">
					<span>Exp.Date(MM/YY)</span>
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

			<Button @click="enableShake" :class="{ shake: shakeEnabled }" label="Confirm"/>
		</form>

		<div class="form-success" v-if="formSubmitted">
			<FormSuccess/>
			<Button :onClick="toContinue" label="Continue"/>
		</div>

	</div>
</template>


<style lang="scss" scoped>
.form-container {
	width: 400px;
	padding: 1em;

	@media screen and (max-width: 950px) {
		padding: 1em;
		width: 100%;
	}

	form {
		display: flex;
		flex-direction: column;
		gap: 1em;

		.shake {
			animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
			transform: translate3d(0, 0, 0);
		}

		@keyframes shake {
			10%,
			90% {
				transform: translate3d(-1px, 0, 0);
			}

			20%,
			80% {
				transform: translate3d(2px, 0, 0);
			}

			30%,
			50%,
			70% {
				transform: translate3d(-4px, 0, 0);
			}

			40%,
			60% {
				transform: translate3d(4px, 0, 0);
			}
		}

		.expiration-details {
			display: flex;

			.expiration {
				display: flex;
				flex: 1;
				flex-direction: column;
				align-items: start;
				padding-right: 1em;

				span {
					text-transform: uppercase;
				}

				.expiration-dates {
					display: flex;
					gap: 0.5em;
				}
			}
		}

		.form-error {
			font-size: 0.7em;
			color: #c50000;
			border: solid 1px #c50000;
			padding: 1em;
			background-color: #fff3f3;
			border-radius: 8px;
		}
	}

	.form-success {
		display: flex;
		flex-direction: column;
		align-items: stretch;
	}
}


</style>
