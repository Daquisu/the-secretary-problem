<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';
	import ResultAndDecision from '../components/ResultAndDecision.svelte';
	import ComponentEvent from '../components/ComponentEvent.svelte';
	import { VisScatter, VisXYContainer, VisLine, VisAxis } from '@unovis/svelte';

	let currentView = [0];
	let decision = '';
	let n_samples = 4;
	let skills = [];
	let visible;
	let win = false;
	let n_wins = 0;
	let n_games = 0;
	let data = [];
	let x = [];
	let y = [];
	let n_ticks = 0;
	let probs = {
		1: 100,
		2: 50,
		3: 50,
		4: 0.4583 * 100,
		5: 0.4333 * 100,
		6: 0.4278 * 100,
		7: 0.4143 * 100,
		8: 0.4098 * 100,
		9: 0.406 * 100,
		10: 0.3987 * 100,
		11: 0.3984 * 100,
		12: 0.3955 * 100,
		13: 0.3923 * 100,
		14: 0.3917 * 100,
		15: 0.3894 * 100,
		16: 0.3881 * 100,
		17: 0.3873 * 100,
		18: 0.3854 * 100,
		19: 0.385 * 100,
		20: 0.3842 * 100,
		30: 0.3786 * 100,
		40: 0.3757 * 100,
		50: 0.3743 * 100,
		60: 0.3732 * 100,
		70: 0.3724 * 100,
		80: 0.3719 * 100,
		90: 0.3714 * 100,
		100: 0.371 * 100,
		1000: 36.81
	};
	let prob = 0.4583 * 100;

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
		win = currentView.length == indexOfMax(skills) + 1;
		n_games++;
		n_wins = win ? n_wins + 1 : n_wins;
		data.push({ x: n_games, y: (100 * n_wins) / n_games });
		x.push(n_games);
		y.push((100 * n_wins) / n_games);
		n_ticks = x.length + 1;
		data = data;
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
		data = [];
		x = [];
		y = [];
		n_games = 0;
		n_wins = 0;
	}
	function onChangeInput() {
		resetGame();
		resetStats();
		if (n_samples in probs) {
			prob = probs[n_samples];
			prob = prob;
			data = data;
		} else if (n_samples < 1001) {
			let x0 = 1;
			let x1 = 1000;
			let diff;
			for (let key in probs) {
				diff = Number(n_samples) - key;
				console.log(n_samples, key, diff);
				console.log(key)
				if (diff > 0 && key > x0) {
					x0 = key;
				} else if (diff < 0 && key < x1) {
					x1 = key;
				}
			}
			console.log(x0, x1);
			prob = probs[x0] + ((probs[x1] - probs[x0]) * (n_samples - x0)) / (x1 - x0);
			data = data;
			console.log('prob2', prob);
		} else {
			prob = (100 * 1) / Math.E;
			prob = prob;
			data = data;
			console.log('prob3', prob);
		}
	}
</script>

<div class="container mx-auto p-8 space-y-8">
	<div class="flex flex-col items-center justify-center">
		<p>Choose the maximum number of applicants to interview.</p>
		<input
			class="chip variant-ringed-secondary text-base"
			id="n_samples"
			type="number"
			name="n_samples"
			bind:value={n_samples}
			on:change={onChangeInput}
		/>
		{#if n_games > 0}
			<p>
				You won {n_wins} of {n_games} games, or {Math.round((10000 * n_wins) / n_games) / 100}% of
				total.
			</p>
			<button class="btn variant-filled-secondary" on:click={resetStats}>Reset stats</button>
			<VisXYContainer {data}>
				<VisScatter x={(d) => d.x} y={(d) => d.y} />
				<VisLine x={(d) => d.x} y={(d) => d.y} />
				<VisLine x={(d) => d.x} y={prob} color="#E2203A" />
				<VisAxis gridLine={false} label="Number of simulations" numTicks={n_ticks} type="x" />
				<VisAxis
					gridLine={false}
					label="Percentage of best choice"
					numTicks={10}
					type="y"
					y={[0, 100]}
				/>
			</VisXYContainer>
		{:else}
			<p>Play a game to see your win rate.</p>
		{/if}
	</div>
	<hr />
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
							<ComponentEvent {component} skill={skills[i]} is_last={i == n_samples - 1} />
						{/each}
					{/if}
				</div>
			{/if}
		{/each}
	</div>
	<div>Click on the image to see the performance.</div>
</div>
