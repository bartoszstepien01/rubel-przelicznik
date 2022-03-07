<script lang="ts">
	import { onMount } from 'svelte';

	interface ForexData {
		USD_RUB: number;
		USD_PLN: number;
		USD_EUR: number;
		USD_GBP: number;
	};

	let pln: string = "";
	let usd: string = "";
	let eur: string = "";
	let gbp: string = "";
	let robux: string = "";
	let vbucks: string = "";
	let riotPoints: string = "";
	let froggs: string = "";
	let mcdPoints: string = "";

	onMount(async() => {
		let response = await fetch('https://free.currconv.com/api/v7/convert?q=USD_RUB,USD_PLN&compact=ultra&apiKey=0fd8d04cf38ccf8e4c84');
		let data: ForexData = await response.json();
		response = await fetch('https://free.currconv.com/api/v7/convert?q=USD_EUR,USD_GBP&compact=ultra&apiKey=0fd8d04cf38ccf8e4c84');
		data = {...data, ...(await response.json())};

		pln = (data.USD_PLN / data.USD_RUB * 100).toFixed(2);
		usd = (1 / data.USD_RUB * 100).toFixed(2);
		eur = (data.USD_EUR / data.USD_RUB * 100).toFixed(2);
		gbp = (data.USD_GBP / data.USD_RUB * 100).toFixed(2);
		robux = (1 / data.USD_RUB * 80).toFixed(2) + '-' + (1 / data.USD_RUB * 100).toFixed(2);
		vbucks = (1 / data.USD_RUB * 125).toFixed(2) + '-' + (1 / data.USD_RUB * 168.75).toFixed(2);
		riotPoints = (1 / data.USD_RUB * 130).toFixed(2) + '-' + (1 / data.USD_RUB * 150).toFixed(2);
		froggs = (data.USD_PLN / data.USD_RUB * 4).toFixed(2);
		mcdPoints = (data.USD_PLN / data.USD_RUB * 10).toFixed(2);
	});
</script>

<svelte:head>
	<title>Przelicznik rubla</title>
</svelte:head>

<div class="flex items-center justify-center h-screen">
	<div class="bg-white rounded-md text-black shadow-sm m-3 sm:m-0">
		<div class="p-4 bg-yellow-400 rounded-t-md shadow-sm">
			<h1 class="text-3xl font-semibold">Przelicznik rubla</h1>
		</div>
		<div class="px-4 py-8">
			<p class="text-lg sm:text-xl text-center font-medium"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">1</span> rubel jest obecnie &quot;warty&quot;</p>
			<div class="grid grid-cols-2 gap-3 mt-8">
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{pln}</span> groszy</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{usd}</span> centów</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{eur}</span> eurocentów</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{gbp}</span> pensów</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{mcdPoints}</span> punktów McD</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{froggs}</span> żappsów</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{robux}</span> Robuxów</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{vbucks}</span> V-dolców</p>
				<p class="text-base sm:text-lg text-center font-medium whitespace-nowrap"><span class="text-white bg-black bg-opacity-75 rounded-sm px-2">{riotPoints}</span> Riot Points</p>
			</div>
		</div>
		<div class="text-center pb-4">
			<p>Dane pochodzą z <a class="text-blue-700 hover:text-blue-900" href="https://currencyconverterapi.com/">currencyconverterapi.com</a></p>
			<p>Слава Україні! 🇺🇦</p>
		</div>
	</div>
</div>

