<script lang="ts">
	import { goto } from '$app/navigation';
	import { supabase } from '$lib/supabaseClient';

	const signout = async () => {
		await supabase.auth.signOut();
		goto('/');
	};
</script>

<button on:click={signout}>sign out</button>
