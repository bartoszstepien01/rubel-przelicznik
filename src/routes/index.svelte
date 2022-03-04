<script lang="ts">
	import { onMount } from 'svelte';
	import moment from 'moment/min/moment-with-locales.js';

	interface ForexData {
		time_last_update_unix: number;
		rates: {
			RUB: number;
			PLN: number;
			EUR: number;
		}
	};

	let lastUpdate: string;
	let pln: string;
	let usd: string;
	let eur: string;
	let robux: string;
	let gems: string;
	let vbucks: string;

	onMount(async() => {
		let response = await fetch('https://open.er-api.com/v6/latest/USD');
		let data: ForexData = await response.json();

		pln = (data.rates.PLN / data.rates.RUB * 100).toFixed(2);
		usd = (1 / data.rates.RUB * 100).toFixed(2);
		eur = (data.rates.EUR / data.rates.RUB * 100).toFixed(2);
		robux = (1 / data.rates.RUB * 80).toFixed(2) + '-' + (1 / data.rates.RUB * 100).toFixed(2);
		gems = (1 / data.rates.RUB * 80).toFixed(2) + '-' + (1 / data.rates.RUB * 140).toFixed(2);
		vbucks = (1 / data.rates.RUB * 125).toFixed(2) + '-' + (1 / data.rates.RUB * 168.75).toFixed(2);

		moment.locale('pl');
		lastUpdate = moment.unix(data.time_last_update_unix).fromNow();
	});
</script>

<svelte:head>
	<title>Przelicznik rubla</title>
</svelte:head>

<div class="flex items-center justify-center h-screen">
	<div class="bg-white rounded-md text-black ">
		<div class="p-4 bg-yellow-400 rounded-t-md">
			<h1 class="text-3xl font-semibold">Przelicznik rubla</h1>
			<p>Ostatnio zaktualizowano: {lastUpdate}</p>
		</div>
		<div class="px-4 py-8">
			<p class="text-xl text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">1</span> rubel jest obecnie &quot;warty&quot;</p>
			<div class="grid grid-cols-2 gap-3 mt-8">
				<p class="text-lg text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{pln}</span> groszy</p>
				<p class="text-lg text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{usd}</span> centów</p>
				<p class="text-lg text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{eur}</span> eurocentów</p>
				<p class="text-lg text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{robux}</span> Robuxów</p>
				<p class="text-lg text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{gems}</span> gemów</p>
				<p class="text-lg text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{vbucks}</span> V-dolców</p>
			</div>
		</div>
		<div class="text-center pb-4">
			<p>Dane pochodzą z <a class="text-blue-500 hover:text-blue-700" href="https://www.exchangerate-api.com">exchangerate-api.com</a></p>
			<p>Слава Україні! 🇺🇦</p>
		</div>
	</div>
</div>

