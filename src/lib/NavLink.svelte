<script lang="ts">
	export let text:string = 'Home';
	export let to:string = '/'

	import {useNavigate} from 'svelte-navigator';

	const navigate = useNavigate();

</script>

<p on:click={()=>{navigate(to)}}
	on:keydown={()=>{navigate(to)}}
	>{text}</p>

<style>
	p {
		color: white;
		font-size: 1.5rem;
		font-weight: 350;
		position: relative;
		cursor: pointer;
		/* border-radius: 0.2rem; */
		border-bottom: 1px solid #ffffff00;
		transition: all 0.5s ease;
	}

	p:hover {
		border-bottom: 1px solid white;
		transition: all 0.5s ease;
	}
</style>