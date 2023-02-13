<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
    function getRandomIntBetweenZeroAndOneHundred() {
		return Math.floor(Math.random() * 101);
	}
    function dispatchAccept() {
		dispatch('decision', 'Accepted');
	}
    function dispatchReject() {
		dispatch('decision', 'Rejected');
	}
</script>

<div class="">
    <div class="title"> Interview Result: {getRandomIntBetweenZeroAndOneHundred()}% </div>
    <div class="flex ">
        <button class="btn bg-tertiary-500" on:click={dispatchAccept}>Accept</button>
        <button class="btn bg-primary-500" on:click={dispatchReject}>Reject</button>
    </div>
    
</div>