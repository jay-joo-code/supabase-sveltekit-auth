<script lang="ts">
	import { dev } from '$app/environment';
	import { supabase } from '$lib/supabaseClient';

	let loading = false;
	let email: string = 'jaehyungjoo1@gmail.com';

	const handleLogin = async () => {
		try {
			loading = true;
			const { error } = await supabase.auth.signInWithOtp({
				email,
				options: {
					emailRedirectTo: dev ? 'http://localhost:5173/' : undefined
				}
			});
			if (error) throw error;
			alert('Check your email for the login link!');
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	};

	async function signInWithGoogle() {
		await supabase.auth.signInWithOAuth({
			provider: 'google'
		});
	}
</script>

<form class="row flex-center flex" on:submit|preventDefault={handleLogin}>
	<div class="col-6 form-widget">
		<h1 class="header">Supabase + SvelteKit</h1>
		<button on:click={signInWithGoogle}>sign in with google</button>
		<p class="description">Sign in via magic link with your email below</p>
		<div>
			<input class="inputField" type="email" placeholder="Your email" bind:value={email} />
		</div>
		<div>
			<input
				type="submit"
				class="button block"
				value={loading ? 'Loading' : 'Send magic link'}
				disabled={loading}
			/>
		</div>
	</div>
</form>
