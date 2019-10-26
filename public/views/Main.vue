<template>
	<div>
		<h1>OrFeed Demo</h1>
		<div id="main">
			<div id="params">
				<div id="input">
					<select v-model="inputAsset" @change="inputChanged">
						<option v-for="asset in assets">
							{{ asset }}
						</option>
					</select>
					<input v-model="inputAmount" @input="inputChanged">
				</div>
				<div id="output">
					<select v-model="outputAsset" @change="inputChanged">
						<option v-for="asset in assets">
							{{ asset }}
						</option>
					</select>
					<span id="output-amount">
						<span v-if="loading">loading…</span>
						<span v-else>{{ outputAmount }}</span>
					</span>
				</div>
			</div>
			<div id="rate">Rate: {{ rate }}</div>
		</div>
	</div>
</template>

<script>
import { ethers } from 'ethers';

import orfeedAbi from '../abi/orfeed.json';

const orfeedAddress = '0x298E0d904954E5076b1836937cB3B3527Adf8783';

const infuraId = '202f7e63f7924de1b214ec587219d93d';
const provider = new ethers.providers.InfuraProvider('homestead', infuraId);

export default {
	data() {
		return {
			inputAsset: 'USD',
			outputAsset: 'JPY',
			inputAmount: '50',
			outputAmount: '0',
			loading: false,
		}
	},
	mounted() {
		this.loadOutputAmount();
	},
	methods: {
		inputChanged() {
			this.loadOutputAmount();
		},
		async loadOutputAmount() {
			this.loading = true;
			const orfeed = new ethers.Contract(orfeedAddress, orfeedAbi, provider);
			const rate = await orfeed.getExchangeRate(this.inputAsset, this.outputAsset, '', this.inputAmount);
			this.loading = false;
			this.outputAmount = rate.toString();
		},
	},
	computed: {
		assets() {
			return ['USD', 'EUR', 'CHF', 'JPY', 'GBP'];
		},
		rate() {
			if (this.loading) {
				return 'loading…';
			}
			const inputAmount = parseFloat(this.inputAmount);
			const outputAmount = parseFloat(this.outputAmount);
			const rate = outputAmount / inputAmount;
			return rate;
		}
	}
}
</script>

<style scoped>
#main {
	height: 70vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

#params {
	display: flex;
}

#rate {
	margin-top: 32px;
}

#input,
#output {
	display: flex;
	flex-direction: column;
}

#output {
	margin-left: 4em;
}

#output-amount {
	margin-top: 4px;
}

select {
	width: 80px;
	font-size: 18px;
}

input {
	width: 80px;
	font-size: 18px;
}
</style>
