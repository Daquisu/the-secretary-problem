<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';
	import ResultAndDecision from '../components/ResultAndDecision.svelte';
	import ComponentEvent from '../components/ComponentEvent.svelte';
	import { InputChip } from '@skeletonlabs/skeleton';

	let currentView = [0];
	let decision = '';
	let n_samples = 4;
	let skills = [];
	let visible;
	let win = false;
	let n_wins = 0;
	let n_games = 0;

	for (let i = 0; i < n_samples; i++) {
		skills.push(Math.floor(Math.random() * 100));
	}
	let components = [{ listeners: { decision: handleMessage }, component: ResultAndDecision }];
	function changeCurrentViewToOne(pos) {
		currentView[pos] = 1;
	}
	function handleMessage(event) {
		if (!visible) {
			decision = event.detail;
			if (decision == 'Rejected') {
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
		win = currentView.length == (indexOfMax(skills) + 1);
		n_games++;
		n_wins = win ? n_wins + 1 : n_wins;
		// TODO(Daquisu): Show other candidates after finishing the game?
		// for (let i = currentView.length; i < n_samples; i++) {
		// 	currentView.push(0);
		// }
	}
	function resetGame() {
		currentView = [0];
		skills = [];
		for (let i = 0; i < n_samples; i++) {
			skills.push(Math.floor(Math.random() * 100));
		}
		visible = false;
	}
	function resetStats() {
		n_games = 0;
		n_wins = 0;
	}
	function onChangeInput() {
		resetGame();
		resetStats();
	}
</script>

<div class="container mx-auto p-8 space-y-8">
	<div class="flex flex-col items-center justify-center">
		<p> Choose the maximum number of applicants to interview.</p>
		<input
			class="chip variant-ringed-secondary text-base"
			id="n_samples"
			type="number"
			name="n_samples"
			bind:value={n_samples}
			on:change={onChangeInput}
		/>
		{#if n_games > 0}
			<p> You won {n_wins} of {n_games} games, or {Math.round(10000*n_wins/n_games)/100}% of total.</p>
			<button class="btn variant-filled-secondary" on:click={resetStats}>Reset stats</button>
		{/if}
	</div>
	<hr/>
	<div class="grid justify-center">
		{#if visible}
			<aside
				class="alert z-50 opacity-100 variant-filled-warning absolute  top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2"
			>
				<div class="alert-message">
					<h3>End of game!</h3>
					<p>
						{'The best guess is ' +
							(indexOfMax(skills) + 1) +
							' with a score of ' +
							skills[indexOfMax(skills)] +
							'.'}
					</p>
					<p>
						{'Your guess was ' +
							currentView.length +
							' with a score of ' +
							skills[currentView.length - 1] +
							'.'}
					</p>
				</div>
				<!-- Actions -->
				<div class="alert-actions">
					<button class="btn variant-filled-primary" on:click={resetGame}>Play again</button>
				</div>
			</aside>
		{/if}
	</div>
	<div class="grid grid-cols-4">
		{#each currentView as cv, i}
			{#if i < n_samples}
				<div
					class="justify-center items-center card card-hover p-0 h-72 w-72 mr-6 mb-6"
					on:click={() => changeCurrentViewToOne(i)}
					on:keypress={() => changeCurrentViewToOne(i)}
				>
					{#if cv == 0}
						<CustomAvatar id={Math.ceil(Math.random() * 70) + 1} />
					{:else}
						{#each components as component}
							<ComponentEvent {component} skill={skills[i]} is_last={i == (n_samples - 1)} />
						{/each}
					{/if}
				</div>
			{/if}
		{/each}
	</div>
	<div>Click on the image to see the performance.</div>
</div>
