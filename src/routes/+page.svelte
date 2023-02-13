<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';
	import ResultAndDecision from '../components/ResultAndDecision.svelte';
	import ComponentEvent from '../components/ComponentEvent.svelte';
	import { InputChip } from '@skeletonlabs/skeleton';

	let currentView = [0];
	let decision = "";
	let n_samples = 4;
	let skills = [];
	let i;
	for (let i = 0; i < n_samples; i++) {
		skills.push(Math.floor(Math.random() * 100));
	}
	let components = [{ listeners: { decision: handleMessage }, component: ResultAndDecision }];
	function changeCurrentViewToOne(pos) {
		currentView[pos] = 1 
	}
	function handleMessage(event) {
		decision = event.detail;
		if (decision == "Rejected") {
			currentView.push(0);
		} else {
			finishGame();
		}
	}
	function indexOfMax(arr) {
		if (arr.length === 0) {
			return -1;
		}

		var max = arr[0];
		var maxIndex = 0;

		for (var i = 1; i < arr.length; i++) {
			if (arr[i] > max) {
				maxIndex = i;
				max = arr[i];
			}
		}

		return maxIndex;
	}
	function finishGame() {
		let bestGuess = indexOfMax(skills) + 1;
		alert("The best guess is " + bestGuess + " with a score of " + skills[bestGuess-1] + ".\nYour quess was " + (currentView.length) + " with a score of " + skills[currentView.length-1] + ".");
		// TODO(Daquisu): Add a button to reset the game.
		// TODO(Daquisu): Customize the alert.
	}
	function resetGame() {
		currentView = [0];
		skills = [];
		for (let i = 0; i < n_samples; i++) {
			skills.push(Math.floor(Math.random() * 100));
		}
	}
</script>
<div class="container mx-auto p-8 space-y-8">
	<h1>Hello Skeleton</h1>
	<p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
	<hr />
	<section class="card p-4">
		<p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
	</section>
	<hr />
	<section class="flex space-x-2">
		<a class="btn variant-filled-primary" href="https://kit.svelte.dev/" target="_blank" rel="noreferrer">SvelteKit</a>
		<a class="btn variant-filled-secondary" href="https://tailwindcss.com/" target="_blank" rel="noreferrer">Tailwind</a>
		<a class="btn variant-filled-tertiary" href="https://github.com/" target="_blank" rel="noreferrer">GitHub</a>
	</section>

	<hr />
	<input class="chip variant-ringed-secondary" id="n_samples" type="number" name="n_samples" bind:value={n_samples} />
	<button class="btn variant-filled-primary" on:click={resetGame}>Reset</button>
	<div> Click on the image to see the performance. </div>
	<div class="grid grid-cols-4"> 
		{#each currentView as cv, i}
			{#if i < n_samples}
				<div class="justify-center items-center card card-hover p-0 h-72 w-72 mr-6 mb-6" on:click={() => changeCurrentViewToOne(i)} on:keypress={() => changeCurrentViewToOne(i)}>
					<!-- Got it from:
						https://svelte.dev/repl/5b495a6d61e64d0cabdb3657f100837c?version=3.18.2  
					No idea why it doesn't work using component[0], though.-->
					{#if cv == 0}
						<CustomAvatar />
					{:else}
						{#each components as component}
							<ComponentEvent {component} skill={skills[i]}/>
						{/each}
					{/if}
				</div>
			{/if}
		{/each}
	</div>
</div>