<script lang="ts">
import Form from "./components/Form.vue"
import {defineComponent} from 'vue'

export default defineComponent({
	components: {
		Form
	},
	data() {
		return {
			cardData: {
				nameValue: "",
				cardNumber: "",
				expDateMM: "",
				expDateYY: "",
				CVC: "",
			},
		}
	},
	methods: {
		handleData(data) {
			this.cardData.cardNumber = data.cardNumber
			this.cardData.nameValue = data.nameValue
			this.cardData.expDateYY = data.expDateYY
			this.cardData.expDateMM = data.expDateMM
			this.cardData.CVC = data.CVC
		},
		isEmpty(value: string) {
			return !value.length;
		}

	}
})

</script>

<template>
	<div class="composition">

		<div class="images">
			<div class="bg-card front">
				<div class="circles">
					<div class="big-circle"></div>
					<div class="small-circle"></div>
				</div>
				<h2>{{ isEmpty(cardData.cardNumber) ? "0000 0000 0000 0000" : cardData.cardNumber }}</h2>

				<div class="details">
					<p>{{ isEmpty(cardData.nameValue) ? "Jane Appleseed" : cardData.nameValue }}</p>
					<div class="date">
						<p>{{ isEmpty(cardData.expDateMM) ? "00" : cardData.expDateMM }}</p>
						/
						<p>{{ isEmpty(cardData.expDateYY) ? "00" : cardData.expDateYY }}</p>
					</div>
				</div>
			</div>

			<div class="bg-card back">
				<p class="cvc">{{ isEmpty(cardData.CVC) ? "000" : cardData.CVC }}</p>
			</div>
		</div>

		<Form @sendData="handleData"/>

	</div>
</template>


<style scoped>
.composition {
	display: flex;
	gap: 15em;
	align-items: center;
}

.images {
	background: center / cover no-repeat url('./assets/bg-main-desktop.png');
	height: 900px;
	width: 600px;
	display: flex;
	position: relative;
}

.bg-card {
	position: absolute;
	box-shadow: rgba(100, 100, 111, 0.2) 0 7px 29px 0;
	border-radius: 10px;
	width: 447px;
	height: 245px;
}

.front {
	background: center / contain no-repeat url('./assets/bg-card-front.png');
	top: 200px;
	left: 250px;
	display: flex;
	flex-direction: column;
	gap: 1em;
	padding: 1em;
}

.details {
	display: flex;
	justify-content: space-between;
}

.circles {
	display: flex;
	margin-bottom: 3em;
	gap: 1em;
	align-items: center;
}

.big-circle {
	border-radius: 50%;
	width: 50px;
	height: 50px;
	background-color: white;
}

.small-circle {
	border-radius: 50%;
	width: 20px;
	height: 20px;
	border: 1px solid white;
}

h2, p, .date {
	color: white;
	text-transform: uppercase;
}

h2 {
	font-size: 32px;
}

.date {
	display: flex;
}

.back {
	background: center / contain no-repeat url('./assets/bg-card-back.png');
	top: 500px;
	left: 340px;
}

.cvc {
	top: 108px;
	left: 340px;
}
</style>
