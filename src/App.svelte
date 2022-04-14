<script>
	import { onMount } from 'svelte';

	let motions = [];

	onMount(() => {
		fetch('http://localhost:3000/motions/', {
			method: 'GET',
			headers: {
				'Content-Type': 'application/json'
			}
		}).then(response => response.json()).then(data => {
			motions = data.motions;
		});
		
	});


	// Vote must be boolean
	const postVote = async (vote, motionId) => {
		console.log(motionId);
		await fetch('http://localhost:3000/motions/vote', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				'Access-Control-Allow-Headers': 'Accept, Accept-Language, Content-Language, Content'
			},
			body: JSON.stringify({
				vote: vote,
				motionId: motionId + 1 // motions ids are starting with 1 but indexes not.
			})
		});

		vote ? motions[motionId].upvote += 1 : motions[motionId].downvote += 1;
	}

</script>

<main>
	{#each motions as motion, index}
	<div>
		<h1>{motion.name}</h1>
		<p>{motion.motionId}</p>

		<h4>Upvote {motion.upvote}</h4>
		<h4>Downvote {motion.downvote}</h4>

		<button on:click={() => postVote(true, index)}>Upvote</button>
		<button on:click={() => postVote(false, index)}>Downvote</button>
	</div>
	{/each}
</main>	

<style>

</style>