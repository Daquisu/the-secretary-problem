<script>
	import CustomAvatar from '../components/CustomAvatar.svelte';	

	function getRandomIntBetweenZeroAndOneHundred() {
		return Math.floor(Math.random() * 101);
	}
	// const views = [new CustomAvatar({id: 0}),
	// 				new CustomAvatar({id: 1})]
	let currentView = 0;
	function toggleView() {
		currentView = currentView == 0 ? 1 : 0
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
		<div class="flex justify-center items-center card card-hover p-0 h-48 w-48 mr-6" on:click={toggleView} on:keypress={toggleView}>
			<svelte:component this={CustomAvatar} id={currentView == 0? 0 : 1}></svelte:component>
		</div>
	</div>
</div>