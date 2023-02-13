<svelte:component this={component.component} { skill } bind:this={instance}/>

<script>
  export let component;
  export let skill;

	let instance;
	
	$: if (instance && component.listeners) {
		for (let [key, listener] of Object.entries(component.listeners)) {
			instance.$on(key, listener);
		}
	}
</script>