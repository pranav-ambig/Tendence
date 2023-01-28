<script lang="ts">
	export let text:string = '';
</script>


<div id="container">
	<p>{text}</p>
	<div id="card"></div>
</div>

<style>
	p {
		color: white;
		font-size: 1.5rem;
		padding: 0rem 2rem 0rem 2rem;
		/* position: absolute; */
	}

	#card {
		width: 100%;
		height: 100%;
		background-color: white;
		opacity: 0.1;
		border-radius: 1rem;
		z-index: -1;
		position: absolute;
		transform: translate(0%, -100%);
		transition: all 0.5s ease;
		/* filter: blur(10px); */
	}
	
	#container {
		/* background-color: grey; */
		position: relative;
		border: 1px solid #ffffff88;
		border-radius: 1rem;
		cursor: pointer;
	}
	
</style>