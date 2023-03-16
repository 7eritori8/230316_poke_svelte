<script lang="ts">
	import { pokemonStore } from '../../module/pokemonStore/pokemonStore';

	import axios from 'axios';
	let selecedID = '700';
	// ポケモンIDリスト
	const pokemonIDArray = [
		25, 89, 91, 94, 133, 134, 135, 136, 143, 195, 196, 197, 202, 470, 471, 700
	];
	$: result = getPokeInfo(selecedID);
	// ポケモンAPIから情報を取得処理
	async function getPokeInfo(selecedID: any) {
		let url = `https://pokeapi.co/api/v2/pokemon/${selecedID}`;
		const res = await axios.get(url);

		return {
			pokeName: res.data.species.name,
			pokeImg: res.data.sprites.front_default
		};
	}
	$: console.log({ $pokemonStore });
	async function onSubmit() {
		let getedPoke = await result;
		pokemonStore.update((n) => {
			n.push(getedPoke);
			return n;
		});
	}
</script>

<div>
	<label for="location" class="block text-sm font-medium leading-6 text-gray-900"
		>ぴ、ぴかちゅう！</label
	>
	<select
		id="location"
		name="location"
		class="mt-2 block w-full rounded-md border-0 py-1.5 pl-3 pr-10 text-gray-900 ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-indigo-600 sm:text-sm sm:leading-6"
		bind:value={selecedID}
	>
		{#each pokemonIDArray as pokemonID}
			<option>{pokemonID}</option>
		{/each}
	</select>
</div>

<div>
	{#await result}
		<p>やせいのポケモンとびだしてきた！</p>
	{:then resultContent}
		<h1>{resultContent.pokeName}</h1>
		<img src={resultContent.pokeImg} alt="" />
	{/await}
</div>
<button
	on:click={() => {
		onSubmit();
	}}
	type="button"
	class="rounded-md bg-indigo-600 py-2.5 px-3.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
	>ゲットだぜ！！</button
>
