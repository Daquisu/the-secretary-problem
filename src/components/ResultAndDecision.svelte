<script>
    function getRandomIntBetweenZeroAndOneHundred() {
		return Math.floor(Math.random() * 101);
	}
</script>

<div class="">
    <div class="title"> Interview Result: {getRandomIntBetweenZeroAndOneHundred()}% </div>
    <div class="flex ">
        <button class="btn bg-tertiary-500">Accept</button>
        <button class="btn bg-primary-500">Reject</button>
    </div>
    
</div>