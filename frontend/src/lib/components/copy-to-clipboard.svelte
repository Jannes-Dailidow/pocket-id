<script lang="ts">
	import * as Tooltip from '$lib/components/ui/tooltip';
	import { LucideCheck } from 'lucide-svelte';
	import type { Snippet } from 'svelte';

	let { value, children }: { value: string; children: Snippet } = $props();

	let open = $state(false);
	let copied = $state(false);

	function onClick() {
		open = true;
		copyToClipboard();
	}

	function onOpenChange(state: boolean) {
		open = state;
		if (!state) {
			copied = false;
		}
	}

	function copyToClipboard() {
		navigator.clipboard.writeText(value);
		copied = true;
		setTimeout(() => onOpenChange(false), 1000);
	}
</script>

<Tooltip.Root closeOnPointerDown={false} {onOpenChange} {open}>
	<Tooltip.Trigger class="text-start" onclick={onClick}>{@render children()}</Tooltip.Trigger>
	<Tooltip.Content onclick={copyToClipboard}>
		{#if copied}
			<span class="flex items-center"><LucideCheck class="mr-1 h-4 w-4" /> Copied</span>
		{:else}
			<span>Click to copy</span>
		{/if}
	</Tooltip.Content>
</Tooltip.Root>
