<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';
	import ResultAndDecision from '../components/ResultAndDecision.svelte';
	import ComponentEvent from '../components/ComponentEvent.svelte';

	let currentView = 0;
	let decision = "";
	let components = [{ listeners: { decision: handleMessage }, props: {}, component: ResultAndDecision }];
	// let rad = { listeners: { decision: handleMessage }, props: {}, component: ResultAndDecision };
	function changeCurrentViewToOne() {
		currentView = 1
	}
	function handleMessage(event) {
		decision = event.detail;
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

	
	<div class="flex"> 
		<div class="flex justify-center items-center card card-hover p-0 h-72 w-72 mr-6" on:click={changeCurrentViewToOne} on:keypress={changeCurrentViewToOne}>
			<!-- Got from:
				https://svelte.dev/repl/5b495a6d61e64d0cabdb3657f100837c?version=3.18.2  
			No idea why it doesn't work using component[0], though.-->
			{#if currentView === 0}
				<CustomAvatar id={0} />
			{:else}
				{#each components as component}
					<ComponentEvent {component}/>
				{/each}
			{/if}
		</div>
	</div>
</div>