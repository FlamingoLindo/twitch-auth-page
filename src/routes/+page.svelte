<script>
	import { onMount } from 'svelte';
	import { writable, get } from 'svelte/store';

	const code = writable('');

	onMount(() => {
		const c = new URL(location.href).searchParams.get('code');
		if (c) code.set(c);
	});

	const copied = writable(false);

	function copyCode() {
		const c = get(code);
		if (c) {
			navigator.clipboard.writeText(c);
			copied.set(true);
			setTimeout(() => copied.set(false), 2000);
		}
	}
</script>

<div
	class="flex min-h-screen items-center justify-center bg-linear-to-b from-purple-600 to-pink-600"
>
	<div class="mx-4 w-full max-w-md rounded-lg bg-white p-8 shadow-2xl">
		<h1 class="mb-6 text-3xl font-bold text-gray-800">Twitch Authorization</h1>

		{#if $code}
			<div class="mb-6 rounded-lg border border-gray-200 bg-gray-50 p-4">
				<p class="mb-3 text-sm font-semibold text-gray-600">Authorization Code:</p>
				<p
					class="rounded border border-gray-300 bg-white p-3 font-mono text-xs break-all text-gray-800"
				>
					{$code}
				</p>
			</div>

			<button
				onclick={copyCode}
				class="w-full transform rounded-lg bg-purple-600 px-4 py-3 font-bold text-white transition duration-200 ease-in-out hover:scale-105 hover:bg-purple-700 active:bg-purple-800"
			>
				{$copied ? '✓ Copied to clipboard!' : '📋 Copy Code'}
			</button>

			<p class="mt-4 text-center text-xs text-gray-500">
				Code copied successfully - you can now close this window
			</p>
		{:else}
			<div class="py-8 text-center">
				<div class="mb-4 inline-block animate-spin">
					<svg
						class="h-12 w-12 text-purple-600"
						fill="none"
						stroke="currentColor"
						viewBox="0 0 24 24"
					>
						<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"
						></circle>
						<path
							class="opacity-75"
							fill="currentColor"
							d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
						></path>
					</svg>
				</div>
				<p class="text-lg font-semibold text-gray-700">Authorizing with Twitch...</p>
				<p class="mt-2 text-sm text-gray-500">
					Please wait while we retrieve your authorization code
				</p>
			</div>
		{/if}
	</div>
</div>
