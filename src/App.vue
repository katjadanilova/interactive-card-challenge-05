<script lang="ts">
import Form from "./components/Form.vue"
import {defineComponent} from 'vue'

export default defineComponent({
	components: {
		Form
	},
	data() {
		return {
			card: {
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
			this.card.cardNumber = data.cardNumber.replace(/(\d{4})/g, '$1 ')
			this.card.nameValue = data.nameValue
			this.card.expDateYY = data.expDateYY
			this.card.expDateMM = data.expDateMM
			this.card.CVC = data.CVC
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
			<div class="cards">
				<div class="bg-card front">
					<div class="circles">
						<div class="big-circle"></div>
						<div class="small-circle"></div>
					</div>
					<h2>{{ isEmpty(card.cardNumber) ? "0000 0000 0000 0000" : card.cardNumber }}</h2>

					<div class="details">
						<p>{{ isEmpty(card.nameValue) ? "Jane Appleseed" : card.nameValue }}</p>
						<div class="date">
							<p>{{ isEmpty(card.expDateMM) ? "00" : card.expDateMM }}</p>
							/
							<p>{{ isEmpty(card.expDateYY) ? "00" : card.expDateYY }}</p>
						</div>
					</div>
				</div>

				<div class="bg-card back">
					<p class="cvc">{{ isEmpty(card.CVC) ? "000" : card.CVC }}</p>
				</div>
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
	align-items: center;
	justify-content: center;
	font-family: 'Space Grotesk', sans-serif;
}

.cards {
	position: relative;
}

.bg-card {
	position: absolute;
	box-shadow: rgba(100, 100, 111, 0.2) 0 7px 29px 0;
	border-radius: 10px;
	width: 430px;
	height: calc(430px * 0.628)
}

.front {
	background: center / cover no-repeat url('./assets/bg-card-front.png');
	top: -250px;
	left: -10px;
	display: flex;
	flex-direction: column;
	gap: 1em;
	padding: 1em;
	justify-content: space-between;
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
	background: center / cover no-repeat url('./assets/bg-card-back.png');
	top: 50px;
	left:60px;
}

.cvc {
	margin-top: 120px;
	margin-left: 350px;
}

@media screen and (max-width: 950px) {
	.composition {
		flex-direction: column;
		gap: 4em;
	}

	.images {
		height: 250px;
		width: 100%;
	}

	.bg-card {
		scale: 0.65;
	}

	.front {
		left: -250px;
		top: -50px;
		z-index: 1;
	}

	.back {
		top: -150px;
		left: -180px;
	}

	.cvc {
		top: 45px;
		left: 350px;
	}


}
</style>
