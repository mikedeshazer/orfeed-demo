<template>
	<div>
		<h1>OrFeed</h1>
		<div id="params">
			<div id="input">
				<select v-model="inputAsset">
					<option v-for="asset in assets">
						{{ asset }}
					</option>
				</select>
				<input v-model="inputAmount">
			</div>
			<div id="output">
				<select v-model="outputAsset">
					<option v-for="asset in assets">
						{{ asset }}
					</option>
				</select>
				<span>{{ outputAmount }}</span>
			</div>
		</div>
	</div>
</template>

<script>
import { ethers } from 'ethers';

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
}

#input,
#output {
	display: flex;
	flex-direction: column;
}

#output {
	margin-left: 2em;
}
</style>
