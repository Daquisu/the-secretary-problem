<script>
	export let component;
	export let skill;
	export let is_last = false;

	let instance;

	$: if (instance && component.listeners) {
		for (let [key, listener] of Object.entries(component.listeners)) {
			instance.$on(key, listener);
		}
	}
</script>

<svelte:component this={component.component} {skill} {is_last} bind:this={instance} />
