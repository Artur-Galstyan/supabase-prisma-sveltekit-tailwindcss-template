<!-- src/routes/+page.svelte -->
<script lang="ts">
	import {Auth, SignIn} from '@supabase/auth-ui-svelte'
	import { ThemeSupa } from '@supabase/auth-ui-shared'
	import {currentUser} from "$lib/currentUser";

	export let data

</script>

<svelte:head>
	<title>User Management</title>
</svelte:head>

{#if $currentUser}
	<div>
		Hi, {$currentUser.user_metadata.full_name}
		<button
				on:click={async() => {
					await data.supabase.auth.signOut()
				}}
				class="btn btn-primary">
			Sign out
		</button>
	</div>
	{:else}

	<div class="row flex-center flex">
		<div class="col-6 form-widget">
			<Auth
					supabaseClient={data.supabase}
					providers={['discord']}
					redirectTo={`${data.url}/auth/callback`}
					showLinks={false}
					appearance={{ theme: ThemeSupa, style: { input: 'color: #fff' } }}
			/>
		</div>
	</div>
{/if}
