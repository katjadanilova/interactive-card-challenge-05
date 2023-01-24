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
					<h1>{{ isEmpty(card.cardNumber) ? "0000 0000 0000 0000" : card.cardNumber }}</h1>

					<div class="details">
						<span>{{ isEmpty(card.nameValue) ? "Jane Appleseed" : card.nameValue }}</span>
						<div class="date">
							<span>{{ isEmpty(card.expDateMM) ? "00" : card.expDateMM }}</span>
							/
							<span>{{ isEmpty(card.expDateYY) ? "00" : card.expDateYY }}</span>
						</div>
					</div>
				</div>

				<div class="bg-card back">
					<span class="cvc">{{ isEmpty(card.CVC) ? "000" : card.CVC }}</span>
				</div>
			</div>
		</div>

		<Form @sendData="handleData"/>

	</div>
</template>


<style lang="scss" scoped>
.composition {
	display: flex;
	gap: 15em;
	align-items: center;

	@media screen and (max-width: 950px) {
		flex-direction: column;
		gap: 4em;
	}

	.images {
		background: center / cover no-repeat url('./assets/bg-main-desktop.png');
		height: 100vh;
		width: 600px;
		display: flex;
		align-items: center;
		justify-content: center;
		font-family: 'Space Grotesk', sans-serif;

		@media screen and (max-width: 950px) {
			height: 250px;
			width: 100%;

			.bg-card {
				scale: 0.7;
			}
		}

		.cards {
			position: relative;
			font-size: 1.2em;

			.bg-card {
				position: absolute;
				box-shadow: rgba(100, 100, 111, 0.2) 0 7px 29px 0;
				border-radius: 10px;
				width: 430px;
				height: calc(430px * 0.628);

				h1 {
					font-size: 1.6em;
					color: white
				}

				&.front {
					background: center / cover no-repeat url('./assets/bg-card-front.png');
					top: -250px;
					left: -10px;
					display: flex;
					flex-direction: column;
					gap: 1em;
					padding: 1em;
					justify-content: space-between;

					@media screen and (max-width: 950px) {
						left: -230px;
						top: -40px;
						z-index: 1;
					}

					.circles {
						display: flex;
						margin-bottom: 3em;
						gap: 1em;
						align-items: center;

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
					}

					.details {
						display: flex;
						justify-content: space-between;

						span, .date {
							color: white;
							text-transform: uppercase;
						}

						.date {
							display: flex;
						}
					}

				}

				&.back {
					background: center / cover no-repeat url('./assets/bg-card-back.png');
					top: 50px;
					left: 60px;
					position: absolute;

					@media screen and (max-width: 950px) {
						left: -200px;
						top: -150px;
					}

					.cvc {
						position: relative;
						color: white;
						top: 120px;
						left: 350px;
					}
				}
			}
		}
	}
}
</style>
