<template>
	<div>
		<h1>OrFeed Demo</h1>
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
				<span id="output-amount">{{ outputAmount }}</span>
			</div>
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
			inputAsset: 'JPY',
			outputAsset: 'USD',
			inputAmount: '100000',
			outputAmount: '0',
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
			const orfeed = new ethers.Contract(orfeedAddress, orfeedAbi, provider);
			const rate = await orfeed.getExchangeRate("JPY", "USD", '', this.inputAmount);
			this.outputAmount = rate.toString();
		},
	},
	computed: {
		assets() {
			return ['USD', 'EUR', 'JPY'];
		}
	}
}
</script>

<style scoped>
#params {
	display: flex;
	height: 70vh;
	justify-content: center;
	align-items: center;
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
	margin-top: 2px;
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
