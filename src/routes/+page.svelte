<script>
	import { page } from '$app/stores';
	import Auth from '$lib/components/Auth.svelte';
	import Dashboard from '$lib/components/Dashboard.svelte';
</script>

<svelte:head>
	<title>Supabase + SvelteKit</title>
	<meta name="description" content="SvelteKit using supabase-js v2" />
</svelte:head>

{#if !$page.data.session}
	<Auth />
{:else}
	<Dashboard session={$page.data.session} />
{/if}
