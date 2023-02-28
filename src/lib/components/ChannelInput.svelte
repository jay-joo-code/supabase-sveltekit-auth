<script lang="ts">
	import { page } from '$app/stores';
	import { supabase } from '$lib/supabaseClient';
	import type { RealtimeChannel } from '@supabase/supabase-js';

	export let channelId: string;
	export let supabaseChannel: RealtimeChannel;

	let message: string = '';

	const handleSubmit = async () => {
		try {
			if ($page.data.session) {
				const { data, error } = await supabase
					.from('messages')
					.insert({
						message,
						channel_id: Number(channelId),
						user_id: $page.data.session?.user?.id
					})
					.select('*, users(*)')
					.single();
				if (error) throw error;
				message = '';
				await supabaseChannel.send({
					type: 'broadcast',
					event: 'new-message',
					payload: { data }
				});
			}
		} catch (error) {}
	};
</script>

<form on:submit|preventDefault={handleSubmit}>
	<input bind:value={message} />
</form>
