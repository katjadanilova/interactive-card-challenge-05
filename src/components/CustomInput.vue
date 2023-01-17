<template>
	<div class="input-field">
		<label>{{ label }}</label>
		<input :maxlength="maxLength" v-model=text :placeholder=placeholder :class="classes" @keyup="formatCard">
		<span v-if="error">{{ error }}</span>
	</div>
</template>

<script lang="ts">
import {defineComponent} from 'vue'

function validateNumbers(value: string): string | null {
	if (!/^(?=.*\d)[\d ]+$/.test(value))
		return 'Wrong format, numbers only';

	return null;
}

export default defineComponent({
	name: "CustomInput",

	props: {
		label: String,
		placeholder: {type: String, required: true},
		kind: {type: String, required: true},
		required: {type: Boolean, required: false, default: true},
		forceShowError: {type: Boolean, default: true}
	},

	data() {
		return {
			text: "",
			pristine: true
		}
	},

	watch: {
		text() {
			this.pristine = false;
			if (!this.error) {
				if (this.kind === "number")
					this.$emit("update:modelValue", this.text.replace(/\s/g, ''))
				this.$emit("update:modelValue", this.text)
			}
		}
	},

	methods: {
		formatCard() {
			if (this.kind === "number") {
				let nn = this.text;
				(nn.length - (nn.split(" ").length - 1)) % 4 === 0 ? (this.text += ' ').trim() : ''
			}
		}
	},

	computed: {
		error(): string | null {
			const showError = this.forceShowError || !this.pristine;
			if (!showError)
				return null;

			if (this.text.length === 0) {
				if (this.required) {
					return "Can't be blank"
				}
			} else if (this.kind === "number") {
				return validateNumbers(this.text);
			}
			return null;
		},
		classes(): any {
			return {
				"is-error": this.error !== null
			}
		},
		maxLength(): number {
			if (this.kind === "number")
				return 23
			if (this.kind === "date")
				return 2
			if (this.kind === "cvc")
				return 3

			return null
		}
	},


	emits: ["update:modelValue"],
});
</script>

<style scoped>
.input-field {
	display: flex;
	flex-direction: column;
	gap: 0.3em;
}

label {
	text-transform: uppercase;
}

input {
	width: 100%;
	font-size: 18px;
	border-radius: 8px;
	padding: 0.5em 1em 0.5em 1em;
	border: solid 1px var(--vt-c-divider-light-1);
	font-family: inherit;

}

.is-error {
	border: solid 1px #c50000;
}

.is-error:focus {
	outline-color: #c50000;
	border-color: transparent;
}

input::placeholder {
	color: var(--color-border-hover)
}

input:focus {
	outline-color: hsl(279, 6%, 55%);
	border-color: transparent;
}

span {
	font-size: 12px;
	color: #c50000;
}


</style>