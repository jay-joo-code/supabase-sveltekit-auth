<script lang="ts">
	import { page } from '$app/stores';
	import ChannelInput from '$lib/components/ChannelInput.svelte';
	import { supabase } from '$lib/supabaseClient';
	import { onMount } from 'svelte';

	type TMessagesResponse = Awaited<ReturnType<typeof getMessages>>;

	let messages: TMessagesResponse['data'] = [];
	const supabaseChannel = supabase.channel($page?.data?.channelId, {
		config: {
			broadcast: {
				self: true
			}
		}
	});

	const getMessages = async (channelId: string) => {
		return await supabase.from('messages').select('*, users(username)').eq('channel_id', channelId);
	};

	$: {
		(async () => {
			messages = (await getMessages($page?.data?.channelId))?.data;
		})();
	}

	const subscribe = () => {
		supabaseChannel
			.on('broadcast', { event: 'new-message' }, ({ payload }) => {
				console.log('payload', payload);
				messages = messages ? [...messages, payload?.data] : [payload?.data];
			})
			.subscribe();
	};

	onMount(() => {
		subscribe();
	});

	$: console.log('messages', messages);
</script>

<div>
	<h2>Messages</h2>
	{#if messages}
		{#each messages as message (message?.id)}
			<div>
				<p>{message?.message}</p>
				{#if !Array.isArray(message?.users)}
					<p>{message?.users?.username}</p>
				{/if}
			</div>
		{/each}
	{/if}
	<ChannelInput channelId={$page?.data?.channelId} {supabaseChannel} />
</div>
