<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';
	import ResultAndDecision from '../components/ResultAndDecision.svelte';
	import ComponentEvent from '../components/ComponentEvent.svelte';

	function getRandomIntBetweenZeroAndOneHundred() {
		return Math.floor(Math.random() * 101);
	}
	let currentView = [0];
	let decision = "";
	let components = [{ listeners: { decision: handleMessage }, component: ResultAndDecision }];
	function changeCurrentViewToOne(pos) {
		currentView[pos] = 1 
	}
	function handleMessage(event) {
		decision = event.detail;
		currentView.push(0);
		console.log(decision);
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

	
	<div class="grid grid-cols-4"> 
		{#each currentView as cv, i}
			<div class="justify-center items-center card card-hover p-0 h-72 w-72 mr-6 mb-6" on:click={() => changeCurrentViewToOne(i)} on:keypress={() => changeCurrentViewToOne(i)}>
				<!-- Got it from:
					https://svelte.dev/repl/5b495a6d61e64d0cabdb3657f100837c?version=3.18.2  
				No idea why it doesn't work using component[0], though.-->
				{#if cv == 0}
					<CustomAvatar id={i} />
				{:else}
					{#each components as component}
						<ComponentEvent {component} skill={getRandomIntBetweenZeroAndOneHundred()}/>
					{/each}
				{/if}
			</div>
		{/each}
	</div>
</div>