<script>
	// Visual component to allow handling XML (save, view, open in new window)
	import { browser } from '$app/environment';

	import SaveXMLFileButton from '$lib/UI/FileButtons/SaveXMLFileButton.svelte';
	import OpenXmlInBrowser from '$lib/UI/FileButtons/OpenXMLInBrowser.svelte';
	import Modal from '$lib/UI/MarkdownModal.svelte';
	let showModal = false;

	export let markdownHelp;
	export let xmlDoc;
	export let title = '';
	export let saveFile = 'data.xml';
	export let message = '';

	$: xmlString = stringifyXmlDoc(xmlDoc);

	function stringifyXmlDoc(_xmlDoc) {
		if (browser && _xmlDoc) {
			let xmlString = new XMLSerializer().serializeToString(_xmlDoc.documentElement);
			return xmlString;
		} else return '';
	}
</script>

<div class="rounded-md bg-white mb-4 text-xs pb-2">
	<!-- Panel Header -->
	<div class="flex border-b justify-between">
		<div>
			{#if title}<p class="p-1 px-2 font-bold text-sm">{title}</p>{/if}
		</div>
		<div>
			<OpenXmlInBrowser {xmlDoc} tabName="preoutput" />

			{#if xmlDoc}
				<SaveXMLFileButton {xmlDoc} fileName={saveFile}>
					<button class="p-1 mr-2">Save</button>
				</SaveXMLFileButton>
			{/if}

			<!-- <button class="p-1 mr-2" on:click={(e) => dispatch('clear')}>Clear</button> -->

			{#if markdownHelp}
				<button
					class="w-4 h-4 mr-2 bg-gray-400 rounded-full text-white text-center text-xs"
					on:click={(e) => (showModal = true)}>?</button
				>
			{/if}
		</div>
	</div>
	<!-- Panel Body -->
	<div class="m-4">
		{#if xmlString}
			<pre class="text-xs h-40 overflow-hidden overflow-y-auto">{xmlString}</pre>
		{:else}
			<div class="h-4">{message}</div>
		{/if}
	</div>
</div>

<Modal bind:showModal {title} markdown={markdownHelp} />
