<script lang="ts">
	import Copy from "./Copy.svelte";
	import Download from "./Download.svelte";
	import { IconButtonWrapper } from "@gradio/atoms";
	import type { CustomButton as CustomButtonType } from "@gradio/utils";

	interface Props {
		value: string;
		language: string;
		buttons?: (string | CustomButtonType)[] | null;
		on_custom_button_click?: ((id: number) => void) | null;
		revealed?: boolean;
	}

	let {
		value,
		language,
		buttons = null,
		on_custom_button_click = null,
		revealed = false
	}: Props = $props();

	let just_copied = $state(false);

	// Devices without real hover (touch, coarse pointers) never get a
	// mouseenter/mouseleave to reveal the toolbar, so it stays always-visible
	// there rather than becoming unreachable.
	const supports_hover =
		typeof window !== "undefined" &&
		window.matchMedia("(hover: hover) and (pointer: fine)").matches;

	let visible = $derived(!supports_hover || revealed || just_copied);
</script>

<div class="toolbar" class:hidden={!visible}>
	<IconButtonWrapper {buttons} {on_custom_button_click}>
		{#if buttons?.some((btn) => typeof btn === "string" && btn === "download")}
			<Download {value} {language} />
		{/if}
		{#if buttons?.some((btn) => typeof btn === "string" && btn === "copy")}
			<Copy {value} bind:copied={just_copied} />
		{/if}
	</IconButtonWrapper>
</div>

<style>
	.toolbar {
		opacity: 1;
		transition: opacity 0.15s ease-in-out;
	}

	.toolbar.hidden {
		opacity: 0;
		pointer-events: none;
	}
</style>
