<script>
	import { createEventDispatcher } from 'svelte';
	export let skill;
	export let is_last;

	const dispatch = createEventDispatcher();

	function dispatchAccept() {
		dispatch('decision', 'Accepted');
	}
	function dispatchReject() {
		dispatch('decision', 'Rejected');
	}
</script>

<div class="w-72 h-72 justify-center items-center flex flex-col">
	<div>Interview Result: {skill}%</div>
	<div>
		<button class="btn bg-tertiary-500" on:click={dispatchAccept}>Accept</button>
		{#if !is_last}
			<button class="btn bg-primary-500" on:click={dispatchReject}>Reject</button>
		{/if}
	</div>
</div>
