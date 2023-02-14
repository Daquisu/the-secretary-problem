<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';
	import ResultAndDecision from '../components/ResultAndDecision.svelte';
	import ComponentEvent from '../components/ComponentEvent.svelte';
	import { InputChip } from '@skeletonlabs/skeleton';

	let currentView = [0];
	let decision = "";
	let n_samples = 4;
	let skills = [];
	let visible;
	for (let i = 0; i < n_samples; i++) {
		skills.push(Math.floor(Math.random() * 100));
	}
	let components = [{ listeners: { decision: handleMessage }, component: ResultAndDecision }];
	function changeCurrentViewToOne(pos) {
		currentView[pos] = 1 
	}
	function handleMessage(event) {
		if (!visible) {
			decision = event.detail;
			if (decision == "Rejected") {
				currentView.push(0);
			} else {
				finishGame();
			}
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
		visible = true;
		// TODO(Daquisu): Show other candidates after finishing the game?
		// for (let i = currentView.length; i < n_samples; i++) {
		// 	currentView.push(0);
		// } 
		// TODO(Daquisu): Add a button to reset the game.
		// TODO(Daquisu): Customize the alert.
	}
	function resetGame() {
		currentView = [0];
		skills = [];
		for (let i = 0; i < n_samples; i++) {
			skills.push(Math.floor(Math.random() * 100));
		}
		visible = false;
	}
</script>
<div class="container mx-auto p-8 space-y-8">
	<!-- <h1>Hello Skeleton</h1>
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

	<hr /> -->
	<input class="chip variant-ringed-secondary" id="n_samples" type="number" name="n_samples" bind:value={n_samples} />
	<button class="btn variant-filled-primary" on:click={resetGame}>Reset</button>
	<div> Click on the image to see the performance. </div>
	<div class="grid justify-center">
		{#if visible}
			<aside class="alert z-50 opacity-100 variant-filled-warning absolute  top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
				<!-- Icon -->
				<!-- <div>(icon)</div> -->
				<!-- Message -->
				<div class="alert-message">
					<h3>End of game!</h3>
					<p>{"The best guess is " + (indexOfMax(skills) + 1) + " with a score of " + skills[indexOfMax(skills)-1] + "."}</p>
					<p>{"Your guess was " + (currentView.length) + " with a score of " + skills[currentView.length-1] + "."}</p>
				</div>
				<!-- Actions -->
				<div class="alert-actions">
					<button class="btn variant-filled-primary" on:click={resetGame}>Reset</button>
				</div>
			</aside>
		{/if}
	</div>
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