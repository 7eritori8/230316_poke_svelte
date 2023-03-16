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
	let getMotion = false;
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

<div class="pokeArea">
	{#await result}
		<p>やせいのポケモンとびだしてきた！</p>
	{:then resultContent}
		<div class="poke-info">
			<div class="poke-info-text-area">
				<p>
					{resultContent.pokeName}
				</p>
			</div>
			<div class="poke-info-img bombRightOut">
				<img src={resultContent.pokeImg} alt="" />
			</div>
		</div>
	{/await}
</div>
<button
	on:click={() => {
		// モンスターボールを投げる
		getMotion = true;
		setTimeout(() => {
			getMotion = false;
		}, 1503);
		// ポケモンを追加
		onSubmit();
	}}
	type="button"
	class="move rounded-md bg-indigo-600 py-2.5 px-3.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
	>ゲットだぜ！！</button
>
<div>
	<a href="/show"
		>手持ちのポケモンを見る<svg
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 24 24"
			fill="currentColor"
			class="w-6 h-6"
		>
			<path
				fill-rule="evenodd"
				d="M7.5 5.25a3 3 0 013-3h3a3 3 0 013 3v.205c.933.085 1.857.197 2.774.334 1.454.218 2.476 1.483 2.476 2.917v3.033c0 1.211-.734 2.352-1.936 2.752A24.726 24.726 0 0112 15.75c-2.73 0-5.357-.442-7.814-1.259-1.202-.4-1.936-1.541-1.936-2.752V8.706c0-1.434 1.022-2.7 2.476-2.917A48.814 48.814 0 017.5 5.455V5.25zm7.5 0v.09a49.488 49.488 0 00-6 0v-.09a1.5 1.5 0 011.5-1.5h3a1.5 1.5 0 011.5 1.5zm-3 8.25a.75.75 0 100-1.5.75.75 0 000 1.5z"
				clip-rule="evenodd"
			/>
			<path
				d="M3 18.4v-2.796a4.3 4.3 0 00.713.31A26.226 26.226 0 0012 17.25c2.892 0 5.68-.468 8.287-1.335.252-.084.49-.189.713-.311V18.4c0 1.452-1.047 2.728-2.523 2.923-2.12.282-4.282.427-6.477.427a49.19 49.19 0 01-6.477-.427C4.047 21.128 3 19.852 3 18.4z"
			/>
		</svg>
	</a>
</div>
<div class="pooke-ball-wrapper">
	<img
		class={getMotion === true ? 'getMotion' : 'img-hidden'}
		class:getMotion
		src="pokeball-g809df6f1b_640.png"
		alt=""
	/>
</div>

<style>
	select {
		margin-bottom: 28px;
	}
	.pokeArea {
		height: 200px;
	}
	img {
		display: inline-block;
	}
	.poke-info-text-area {
		margin: 0px 160px 0 auto;
		border: 1px solid black;
		height: 57px;
		width: 130px;
		padding-left: 6px;
	}
	.poke-info-text-area p {
		text-align: left;
	}
	.poke-info-img {
		text-align: right;
	}
	.pooke-ball-wrapper {
		position: absolute;
		right: 280px;
		bottom: 20px;
		width: 30px;
	}
	.img-hidden {
		display: none;
		width: 30px;
	}
	button {
		margin-bottom: 16px;
	}
	svg {
		display: inline;
	}
	.getMotion {
		width: 30px;
		animation-name: stylie-transform-keyframes;
		animation-duration: 1000ms;
		animation-delay: 0ms;
		animation-fill-mode: forwards;
		animation-timing-function: linear;
		transform-origin: 0 0;
	}
	@keyframes stylie-transform-keyframes {
		0% {
			transform: translate(0px, 0px) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg)
				translate(-50%, -50%);
			animation-timing-function: cubic-bezier(0.25, 0.25, 0.75, 0.75);
		}
		60.33% {
			transform: translate(148px, -134px) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg)
				translate(-50%, -50%);
			animation-timing-function: cubic-bezier(0.25, 0.25, 0.75, 0.75);
			opacity: 1;
		}
		70% {
			transform: translate(173px, -140px) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg)
				translate(-50%, -50%);
			animation-timing-function: cubic-bezier(0.25, 0.25, 0.75, 0.75);
			opacity: 1;
		}
		90% {
			transform: translate(223px, -149px) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg)
				translate(-50%, -50%);
			animation-timing-function: cubic-bezier(0.25, 0.25, 0.75, 0.75);
			opacity: 1;
		}
		100% {
			transform: translate(243px, -150px) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg)
				translate(-50%, -50%);
			animation-timing-function: cubic-bezier(0.25, 0.25, 0.75, 0.75);
			opacity: 0;
		}
	}
</style>
