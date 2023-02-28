<script lang="ts">
	import { supabase } from '$lib/supabaseClient';
	import { onMount } from 'svelte';

	type TChannelsResponse = Awaited<ReturnType<typeof getChannels>>;

	let channels: TChannelsResponse['data'] = [];

	const getChannels = async () => {
		return await supabase.from('channels').select();
	};

	onMount(async () => {
		channels = (await getChannels())?.data;
	});
</script>

<div>
	{#if channels}
		{#each channels as channel (channel?.id)}
			<a href="/channel/{channel?.id}">
				<div>
					<button># {channel?.slug}</button>
				</div>
			</a>
		{/each}
	{/if}
</div>
