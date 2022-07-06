<script>
	import { onMount } from 'svelte';

	let movies = [];
	let searchValue = '';
	let searchFocus;

	onMount(() => {
		searchFocus.focus();
	});

	async function movieData() {
		let url = `https://api.themoviedb.org/3/search/movie?api_key=1bd73ac48dac8ac96d592f6c8e119f90&language=en-US&query=${searchValue}&page=1&include_adult=false`;
		let res = await fetch(url);
		let data = await res.json();
		movies = data.results;
	}
</script>

<main class="text-center text-white">
	<div class="mb-10 py-8 bg-slate-800">
		<h1 class="text-5xl mb-6 font-bold">Movies</h1>

		<input
			type="text"
			class="rounded-lg px-2 outline-none focus:outline-yellow-200 text-black"
			placeholder="Searh for a movie title..."
			bind:value={searchValue}
			bind:this={searchFocus}
			on:input={movieData}
		/>
		<p class="mt-4">
			This product uses the TMDb API but is not endorsed or certified by <a
				href="https://www.themoviedb.org/"
				class="underline">TMDb</a
			>.
		</p>
	</div>

	{#if !movies}
		<div />
	{:else}
		<div class="container mx-auto grid grid-cols-5">
			{#each movies as movie (movie.id)}
				<div class="items-center p-4 transition ease-in hover:scale-105 hover:shadow-2xl">
					<div>
						<img
							src="https://image.tmdb.org/t/p/w200{movie.poster_path}"
							alt="movie poster"
							class="inline-block"
						/>
					</div>

					<div class="flex flex-col prose">
						<h3>{movie.title}</h3>
						<h4>
							<svg
								class="w-6 h-6 inline-block"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								xmlns="http://www.w3.org/2000/svg"
								><path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M11.049 2.927c.3-.921 1.603-.921 1.902 0l1.519 4.674a1 1 0 00.95.69h4.915c.969 0 1.371 1.24.588 1.81l-3.976 2.888a1 1 0 00-.363 1.118l1.518 4.674c.3.922-.755 1.688-1.538 1.118l-3.976-2.888a1 1 0 00-1.176 0l-3.976 2.888c-.783.57-1.838-.197-1.538-1.118l1.518-4.674a1 1 0 00-.363-1.118l-3.976-2.888c-.784-.57-.38-1.81.588-1.81h4.914a1 1 0 00.951-.69l1.519-4.674z"
								/></svg
							>{movie.vote_average}
						</h4>
					</div>
				</div>
			{/each}
		</div>
	{/if}
</main>
