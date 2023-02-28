<script lang="ts">
	import { supabase } from '$lib/supabaseClient';
	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';
	import '$lib/styles.css';
	import { page } from '$app/stores';
	import Channels from '$lib/components/Channels.svelte';
	import Signout from '$lib/components/Signout.svelte';

	onMount(() => {
		const {
			data: { subscription }
		} = supabase.auth.onAuthStateChange(() => {
			invalidate('supabase:auth');
		});

		return () => {
			subscription.unsubscribe();
		};
	});
</script>

<div class="container" style="padding: 50px 0 100px 0">
	{#if $page.data.session}
		<Signout />
		<Channels />
	{/if}
	<slot />
</div>
