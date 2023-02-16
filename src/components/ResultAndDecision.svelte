<script>
	import { createEventDispatcher } from 'svelte';
	export let skill;
	export let is_last;
	let already_clicked = false;

	const dispatch = createEventDispatcher();

	function dispatchAccept() {
		if (!already_clicked) {
			dispatch('decision', 'Accepted');
			already_clicked = true;
		}
	}
	function dispatchReject() {
		if (!already_clicked) {
			dispatch('decision', 'Rejected');
			already_clicked = true;
		}
	}
</script>

<div class="w-72 h-72 justify-center items-center flex flex-col">
	<div class="w-48 h-48 flex flex-col justify-between items-center">
		<h2>Interview Result: {skill}%</h2>
		<div>
			<button class="btn bg-tertiary-500" on:click={dispatchAccept}>Accept</button>
			{#if !is_last}
				<button class="btn bg-primary-500" on:click={dispatchReject}>Reject</button>
			{/if}
		</div>
	</div>
</div>
