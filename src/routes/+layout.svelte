<script lang="ts">
    import "../app.css";
    import { invalidate } from '$app/navigation'
	import { onMount } from 'svelte'
    import {currentUser} from "$lib/currentUser";
    import {page} from "$app/stores";

	export let data

	const { supabase, session } = data
    // $: ({ supabase, session } = data)

	onMount(() => {
		const { data } = supabase.auth.onAuthStateChange((event, _session) => {
			if (_session?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth')
                $currentUser = _session?.user;
			}
		})

		return () => data.subscription.unsubscribe()
	})
    $currentUser = $page.data.session?.user;
</script>

<slot />