<template>
	<div>
		<h1>OrFeed</h1>
		<div id="params">
			<div id="input">
				<input v-model="inputAsset">
				<input v-model="inputAmount">
			</div>
			<div id="output">
				<input v-model="outputAsset">
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
